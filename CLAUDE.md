# CLAUDE.md

Instructions for creating bdg tldr pages.

## tldr Page Format

```markdown
# command-name

> Short description.
> Additional info.
> More information: <https://url>.

- Example description:

`command {{arg}} --flag {{value}}`
```

- Arguments in `{{double braces}}` become interactive prompts in tldr++
- Max 8 examples per page
- Keep descriptions concise (one line)

## Page Naming Convention

| Command | Filename |
|---------|----------|
| `bdg` | `bdg.md` |
| `bdg dom` | `bdg-dom.md` |
| `bdg cdp Page` | `bdg-cdp-Page.md` |
| `bdg cdp Runtime` | `bdg-cdp-Runtime.md` |

Spaces become dashes. CDP domain names preserve case.

## Exploring CDP Domains

```bash
# List all 53 CDP domains
bdg cdp --list

# List methods in a domain
bdg cdp Page --list
bdg cdp Runtime --list
bdg cdp Network --list

# Get method count
bdg cdp Page --list 2>/dev/null | jq '.count'

# List method names
bdg cdp Page --list 2>/dev/null | jq -r '.methods[].name' | sort

# Describe a specific method (full schema)
bdg cdp Page.navigate --describe
bdg cdp Runtime.evaluate --describe

# Search across all domains
bdg cdp --search cookie
bdg cdp --search screenshot
```

## Creating a CDP Domain Page

1. **Explore the domain:**
   ```bash
   bdg cdp <Domain> --list 2>/dev/null | jq '.description'
   bdg cdp <Domain> --list 2>/dev/null | jq -r '.methods[].name' | sort
   ```

2. **Identify key methods** (most commonly used):
   - Page: navigate, reload, captureScreenshot, printToPDF
   - Runtime: evaluate, callFunctionOn, getProperties
   - Network: enable, getCookies, setCacheDisabled, setBlockedURLs
   - DOM: getDocument, querySelector, getOuterHTML

3. **Get method signatures:**
   ```bash
   bdg cdp Page.navigate --describe
   ```

4. **Create the page** at `pages/common/bdg-cdp-<Domain>.md`

5. **Symlink to local tldr:**
   ```bash
   ln -sf $(pwd)/pages/common/bdg-cdp-<Domain>.md ~/.local/share/tldr/pages/common/
   ```

6. **Test:**
   ```bash
   tldr bdg cdp <Domain>
   ```

## Priority CDP Domains

Create pages for these domains first (most useful):

| Domain | Methods | Description |
|--------|---------|-------------|
| Page | 61 | Navigation, screenshots, PDF, lifecycle |
| Runtime | 27 | JavaScript execution, object inspection |
| Network | 35 | Cookies, cache, headers, blocking |
| DOM | 45 | Document tree, selectors, HTML |
| Emulation | 25 | Device metrics, geolocation, touch |
| Input | 8 | Mouse, keyboard, touch events |
| Performance | 4 | Metrics collection |
| Storage | 15 | Cookies, localStorage, indexedDB |

## Example: Creating Runtime Domain Page

```bash
# 1. Explore
bdg cdp Runtime --list 2>/dev/null | jq '.description'
# "Runtime domain exposes JavaScript runtime..."

bdg cdp Runtime --list 2>/dev/null | jq -r '.methods[].name' | sort
# awaitPromise, callFunctionOn, evaluate, getProperties, ...

# 2. Check key method signatures
bdg cdp Runtime.evaluate --describe

# 3. Create pages/common/bdg-cdp-Runtime.md with common patterns

# 4. Symlink and test
ln -sf $(pwd)/pages/common/bdg-cdp-Runtime.md ~/.local/share/tldr/pages/common/
tldr bdg cdp Runtime
```

## After Creating Pages

```bash
# Commit and push
git add -A
git commit -m "Add bdg cdp <Domain> tldr page"
git push
```

## Symlink All Pages

```bash
for f in pages/common/bdg*.md; do
  ln -sf "$(pwd)/$f" ~/.local/share/tldr/pages/common/
done
```
