# bdg-tldr-pages

Community-driven help pages for [bdg](https://github.com/anthropics/browser-debugger-cli) - Browser automation via Chrome DevTools Protocol.

## Usage with tldr++

```bash
# Clone to local tldr cache
cp -r pages/common/* ~/.local/share/tldr/pages/common/

# Or symlink
ln -s $(pwd)/pages/common/bdg*.md ~/.local/share/tldr/pages/common/

# Then use
tldr bdg
tldr bdg-dom
tldr bdg-cdp
```

## Pages

| Page | Description |
|------|-------------|
| `bdg` | Main command - session management |
| `bdg-dom` | DOM inspection and manipulation |
| `bdg-dom-a11y` | Accessibility tree queries |
| `bdg-dom-form` | Form discovery and interaction |
| `bdg-cdp` | Direct CDP protocol access |
| `bdg-network` | Network request inspection |
| `bdg-console` | Console message inspection |

## Page Format

Pages follow the [tldr-pages specification](https://github.com/tldr-pages/tldr/blob/main/CONTRIBUTING.md):

```markdown
# command-name

> Short description.
> More information: <https://url>.

- Example description:

`command {{argument}}`
```

Arguments in `{{double braces}}` are interactive prompts in tldr++.

## Contributing

1. Follow the tldr-pages format
2. Keep examples practical and common
3. Use `{{arg}}` for user-provided values
4. Test with `tldr <page-name>`

## License

MIT
