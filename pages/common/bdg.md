# bdg

> Browser automation via Chrome DevTools Protocol.
> Start sessions, inspect DOM, network, console, execute raw CDP commands.
> See also: `bdg dom`, `bdg cdp`, `bdg network`, `bdg console`.
> More information: <https://github.com/anthropics/browser-debugger-cli>.

- Start a session with a URL:

`bdg {{https://example.com}}`

- Start headless with custom Chrome flags:

`bdg --headless {{url}} --chrome-flags="{{--ignore-certificate-errors}}"`

- DOM: query, click, fill, screenshot:

`bdg dom {{query|click|fill|screenshot}} {{selector}}`

- CDP: execute any Chrome DevTools Protocol command (53 domains, 300+ methods):

`bdg cdp {{Runtime.evaluate|Page.navigate|Network.getCookies}} --params '{{...}}'`

- Inspect network requests or console messages:

`bdg {{network|console}} list`

- Check status, preview data, or stop session:

`bdg {{status|peek|stop}}`

- Discover CDP domains and methods:

`bdg cdp --list` or `bdg cdp {{Network}} --list` or `bdg cdp --search {{cookie}}`

- Cleanup stale session or kill all Chrome:

`bdg cleanup {{--force|--aggressive}}`
