# bdg cdp Page

> CDP Page domain - navigation, screenshots, PDF, lifecycle.
> 61 methods for page control and capture.
> More information: <https://chromedevtools.github.io/devtools-protocol/tot/Page>.

- Navigate to a URL:

`bdg cdp Page.navigate --params '{"url": "{{https://example.com}}"}'`

- Reload the page (optionally bypass cache):

`bdg cdp Page.reload --params '{"ignoreCache": {{true|false}}}'`

- Take a screenshot (returns base64):

`bdg cdp Page.captureScreenshot --params '{"format": "{{png|jpeg}}", "quality": {{90}}}'`

- Save page as PDF:

`bdg cdp Page.printToPDF --params '{"printBackground": true}'`

- Handle JavaScript dialog (alert/confirm/prompt):

`bdg cdp Page.handleJavaScriptDialog --params '{"accept": {{true|false}}, "promptText": "{{input}}"}'`

- Set page HTML content directly:

`bdg cdp Page.setDocumentContent --params '{"frameId": "{{main}}", "html": "{{<html>...</html>}}"}'`

- List all Page methods:

`bdg cdp Page --list`

- Describe a specific method:

`bdg cdp Page.{{navigate|captureScreenshot|printToPDF}} --describe`
