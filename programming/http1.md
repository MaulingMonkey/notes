# Quick Overview
HTTP 1.x headers are text based.  All EOLs are `\r\n`.  Request and response headers are both ended with a blank line, even if not followed by data (e.g. `\r\n\r\n`).  The request/response bodies following said headers, if any, may be binary data.  See <https://developer.mozilla.org/en-US/docs/Web/HTTP/Resources_and_specifications> for actual specs.  A minimal reasonable request and response for `http://maulingmonkey.com/brainfuck/` might look like:

### Request
```http
GET /brainfuck/ HTTP/1.1
Host: maulingmonkey.com

```

### Response
```html
HTTP/1.0 200 OK
Content-Length: 11556
Content-Type: text/html; charset=utf-8

<!DOCTYPE html>
<html><head>
	<title>MMIDE - Brainfuck Demo</title>
	<link rel="stylesheet" href="mmide.css" type="text/css" />
	[...the other 11556 bytes of the HTML page...]
```

In reality, this will probably be served over HTTP 2+, the content will be compressed, various additional optional headers will be provided by both the browser and the responding server (including headers to allow the content to be gzip compressed), caching may have happened, etc. etc. etc. - but this serves as a minimal example of what a "hello world" http client or server must aspire to.
