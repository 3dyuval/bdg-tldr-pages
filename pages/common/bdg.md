# bdg

> Browser automation via Chrome DevTools Protocol.
> Start sessions, inspect DOM, network, console, execute raw CDP commands.
> More information: <https://github.com/anthropics/browser-debugger-cli>.

- Start a session with a URL:

`bdg {{https://example.com}}`

- Start in headless mode (no visible browser):

`bdg --headless {{url}}`

- Start with custom Chrome flags (e.g., ignore SSL errors):

`bdg {{url}} --chrome-flags="{{--ignore-certificate-errors}}"`

- Check session status:

`bdg status`

- Preview collected data without stopping:

`bdg peek`

- Stop session and save telemetry to ~/.bdg/session.json:

`bdg stop`

- Force cleanup stale session:

`bdg cleanup --force`

- Kill all Chrome processes (aggressive cleanup):

`bdg cleanup --aggressive`
