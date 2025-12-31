# bdg console

> Inspect browser console messages.
> View logs, errors, warnings collected during the session.
> More information: <https://github.com/anthropics/browser-debugger-cli>.

- Show all console messages:

`bdg console`

- Show console messages as JSON:

`bdg console --json`

- Show last N console messages:

`bdg peek --console --last {{10}}`

- Continuously monitor console (like tail -f):

`bdg tail --console`

- Show console message details by index:

`bdg details console {{index}}`

- Filter by message level via CDP:

`bdg cdp Runtime.evaluate --params '{"expression": "console.{{log|warn|error}}(\"test\")"}'`
