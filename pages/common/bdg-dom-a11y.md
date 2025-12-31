# bdg dom a11y

> Accessibility tree inspection and semantic queries.
> Query elements by ARIA role, name, and properties.
> More information: <https://github.com/anthropics/browser-debugger-cli>.

- Dump full accessibility tree:

`bdg dom a11y tree`

- Query elements by role:

`bdg dom a11y query "role:{{button}}"`

- Query elements by accessible name:

`bdg dom a11y query "name:{{Submit}}"`

- Query with wildcards:

`bdg dom a11y query "name:*{{search}}*"`

- Describe accessibility properties of an element:

`bdg dom a11y describe "{{selector}}"`

- Describe by index from previous query:

`bdg dom a11y describe {{0}}`

- Quick search (auto-detects pattern type):

`bdg dom a11y "{{role:button}}"`
