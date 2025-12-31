# TLDR Adapter Plan

Proposal to auto-generate tldr pages from bdg's existing schema infrastructure.

## Current Architecture

```
devtools-protocol (npm package)
├── json/browser_protocol.json    # 47 domains (Page, Network, DOM, etc.)
└── json/js_protocol.json         # 6 domains (Runtime, Debugger, etc.)
         │
         ▼
src/cdp/protocol.ts               # Loads & merges protocols
         │
         ▼
src/cdp/schema.ts                 # Builds structured schemas
├── getMethodSchema()             # Single method schema
├── getDomainMethods()            # All methods in domain
├── getAllDomainSummaries()       # All 53 domains
└── searchMethods()               # Search across all
         │
         ▼
src/commands/cdp.ts               # CLI handlers
├── --list                        # List domains/methods
├── --describe                    # Method schema + example
└── --search                      # Search methods
```

## Schema Structure

Each method already has all fields needed for tldr:

```typescript
interface MethodSchema {
  name: string;           // "Page.navigate"
  domain: string;         // "Page"
  method: string;         // "navigate"
  description?: string;   // "Navigates current page..."
  experimental?: boolean;
  deprecated?: boolean;
  parameters: ParameterSchema[];
  returns: ReturnSchema[];
  example?: {
    command: string;      // "bdg cdp Page.navigate --params '...'"
    params?: Record<string, unknown>;
  };
}
```

## Proposed Adapter

### Location

```
bdg/
├── src/
│   └── cdp/
│       ├── schema.ts         # Existing
│       └── tldr-generator.ts # NEW - tldr page generator
├── scripts/
│   └── generate-tldr.ts      # NEW - CLI script
└── bdg-tldr-pages/           # Submodule - output target
    └── pages/common/
```

### Generator Script

```typescript
// scripts/generate-tldr.ts
import { getAllDomainSummaries, getDomainMethods } from '@/cdp/schema.js';

interface TldrPage {
  filename: string;
  content: string;
}

function generateDomainPage(domainName: string): TldrPage {
  const methods = getDomainMethods(domainName);
  const topMethods = selectTopMethods(methods, 8); // Max 8 examples

  const content = `# bdg cdp ${domainName}

> CDP ${domainName} domain - ${getDomainDescription(domainName)}.
> ${methods.length} methods available.
> More information: <https://chromedevtools.github.io/devtools-protocol/tot/${domainName}>.

${topMethods.map(m => formatMethod(m)).join('\n\n')}

- List all ${domainName} methods:

\`bdg cdp ${domainName} --list\`

- Describe a specific method:

\`bdg cdp ${domainName}.{{method}} --describe\`
`;

  return {
    filename: `bdg-cdp-${domainName}.md`,
    content
  };
}

function selectTopMethods(methods: MethodSchema[], max: number): MethodSchema[] {
  // Priority: non-deprecated, non-experimental, commonly used
  return methods
    .filter(m => !m.deprecated)
    .sort((a, b) => {
      // Prefer non-experimental
      if (a.experimental !== b.experimental) return a.experimental ? 1 : -1;
      // Prefer methods with fewer required params (simpler)
      return a.parameters.filter(p => p.required).length -
             b.parameters.filter(p => p.required).length;
    })
    .slice(0, max - 2); // Leave room for --list and --describe
}

function formatMethod(method: MethodSchema): string {
  const desc = method.description?.split('.')[0] || method.method;
  return `- ${desc}:

\`${method.example?.command || `bdg cdp ${method.name}`}\``;
}
```

### Integration Options

#### Option A: Build-time Generation (Recommended)

Add to `package.json`:

```json
{
  "scripts": {
    "generate:tldr": "tsx scripts/generate-tldr.ts",
    "build": "tsc && npm run generate:tldr"
  }
}
```

Pages regenerate on every build, staying in sync with protocol version.

#### Option B: npm postinstall Hook

```json
{
  "scripts": {
    "postinstall": "npm run generate:tldr"
  }
}
```

#### Option C: GitHub Action

```yaml
# .github/workflows/update-tldr.yml
on:
  push:
    paths:
      - 'package.json'  # devtools-protocol version change

jobs:
  update-tldr:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          submodules: true
      - run: npm ci && npm run generate:tldr
      - run: |
          cd bdg-tldr-pages
          git add -A
          git commit -m "Update tldr pages for protocol $(jq -r '.dependencies["devtools-protocol"]' ../package.json)"
          git push
```

## Priority Domains

Generate pages for these first (most useful for agents):

| Priority | Domain | Methods | Key Operations |
|----------|--------|---------|----------------|
| 1 | Page | 61 | navigate, reload, captureScreenshot, printToPDF |
| 2 | Runtime | 27 | evaluate, callFunctionOn, getProperties |
| 3 | Network | 35 | getCookies, setCacheDisabled, setBlockedURLs |
| 4 | DOM | 45 | getDocument, querySelector, getOuterHTML |
| 5 | Input | 8 | dispatchMouseEvent, dispatchKeyEvent |
| 6 | Emulation | 25 | setDeviceMetricsOverride, setGeolocation |
| 7 | Storage | 15 | getCookies, clearCookies, clearDataForOrigin |
| 8 | Performance | 4 | enable, getMetrics |

## CLI Command Pages

Also generate from `bdg --help --json`:

```typescript
function generateCliPages(): TldrPage[] {
  const help = JSON.parse(execSync('bdg --help --json').toString());

  return help.command.subcommands.map(cmd => ({
    filename: `bdg-${cmd.name}.md`,
    content: formatCliCommand(cmd)
  }));
}
```

## Implementation Steps

1. [ ] Create `src/cdp/tldr-generator.ts` with generation logic
2. [ ] Create `scripts/generate-tldr.ts` CLI entry point
3. [ ] Add `generate:tldr` npm script
4. [ ] Generate priority domains (Page, Runtime, Network, DOM)
5. [ ] Add CLI command pages (dom, network, console, peek, etc.)
6. [ ] Set up GitHub Action for auto-updates
7. [ ] Update bdg-tldr-pages submodule

## Symlink Management

After generation:

```bash
# Symlink all pages to local tldr
for f in bdg-tldr-pages/pages/common/bdg*.md; do
  ln -sf "$(realpath $f)" ~/.local/share/tldr/pages/common/
done
```

Or add to shell rc:

```bash
# ~/.zshrc or ~/.bashrc
export TLDR_PAGES_PATH="$HOME/path/to/bdg-tldr-pages/pages:$TLDR_PAGES_PATH"
```

## Versioning

Track protocol version in generated pages:

```markdown
# bdg cdp Page

> CDP Page domain (protocol v1.3).
> ...
```

This helps identify stale pages when protocol updates.
