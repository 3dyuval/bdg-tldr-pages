# bdg dom

> DOM inspection and manipulation via Chrome DevTools Protocol.
> Query elements, click, fill forms, take screenshots.
> More information: <https://github.com/anthropics/browser-debugger-cli>.

- Query elements by CSS selector:

`bdg dom query "{{selector}}"`

- Get semantic accessibility info for an element (70-99% fewer tokens):

`bdg dom get "{{selector}}"`

- Get raw HTML of an element:

`bdg dom get "{{selector}}" --raw`

- Click an element:

`bdg dom click "{{selector}}"`

- Fill a form field:

`bdg dom fill "{{selector}}" "{{value}}"`

- Submit a form:

`bdg dom submit "{{button_selector}}"`

- Take a screenshot:

`bdg dom screenshot {{path.png}}`

- Take a screenshot of a specific element:

`bdg dom screenshot {{path.png}} --selector "{{selector}}"`

- Evaluate JavaScript in page context:

`bdg dom eval "{{document.title}}"`
