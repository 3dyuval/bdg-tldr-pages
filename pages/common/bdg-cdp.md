# bdg cdp

> Direct Chrome DevTools Protocol access (53 domains, 300+ methods).
> Execute any CDP command with full parameter control.
> More information: <https://chromedevtools.github.io/devtools-protocol/>.

- List all CDP domains:

`bdg cdp --list`

- List methods in a domain:

`bdg cdp {{Network}} --list`

- Describe a method with full schema:

`bdg cdp {{Network.getCookies}} --describe`

- Search for methods by keyword:

`bdg cdp --search {{cookie}}`

- Execute a CDP method:

`bdg cdp {{Runtime.evaluate}} --params '{"expression": "{{document.title}}", "returnByValue": true}'`

- Navigate to a URL:

`bdg cdp Page.navigate --params '{"url": "{{https://example.com}}"}'`

- Take a screenshot (returns base64):

`bdg cdp Page.captureScreenshot --params '{"format": "png"}' | jq -r '.data' | base64 -d > {{screenshot.png}}`

- Get all cookies:

`bdg cdp Network.enable && bdg cdp Network.getCookies`

- Disable cache:

`bdg cdp Network.enable && bdg cdp Network.setCacheDisabled --params '{"cacheDisabled": true}'`
