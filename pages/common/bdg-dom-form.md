# bdg dom form

> Discover forms with semantic labels, values, and validation state.
> Inspect form fields, their types, and required status.
> More information: <https://github.com/anthropics/browser-debugger-cli>.

- Discover primary form on page:

`bdg dom form`

- Show all forms on page:

`bdg dom form --all`

- Quick scan (field names, types, required only):

`bdg dom form --brief`

- Output as JSON:

`bdg dom form --json`

- Fill a form field:

`bdg dom fill "{{selector}}" "{{value}}"`

- Submit a form:

`bdg dom submit "{{button_selector}}"`

- Submit and wait for navigation:

`bdg dom submit "{{button_selector}}" --wait-navigation`
