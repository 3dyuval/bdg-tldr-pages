# bdg network

> Inspect network requests and resources.
> View collected HTTP traffic during the session.
> More information: <https://github.com/anthropics/browser-debugger-cli>.

- List all network requests:

`bdg network list`

- List requests as JSON:

`bdg network list --json`

- Show request details by ID:

`bdg details network {{request_id}}`

- Filter requests by resource type:

`bdg peek --network --type {{XHR,Fetch}}`

- Show last N network requests:

`bdg peek --network --last {{10}}`

- Continuously monitor network (like tail -f):

`bdg tail --network`

- Disable cache via CDP:

`bdg cdp Network.enable && bdg cdp Network.setCacheDisabled --params '{"cacheDisabled": true}'`

- Block URL patterns via CDP:

`bdg cdp Network.enable && bdg cdp Network.setBlockedURLs --params '{"urls": ["{{*analytics*}}", "{{*tracking*}}"]}'`
