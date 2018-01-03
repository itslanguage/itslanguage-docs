# Cross Origin Resource Sharing

The API supports Cross Origin Resource Sharing (CORS) for AJAX requests.
you can read the [CORS W3C working draft](http://www.w3.org/TR/cors), or
[this intro](http://code.google.com/p/html5security/wiki/CrossOriginRequestSecurity) from the
HTML 5 Security Guide.

Here's a sample request sent from a browser hitting
`http://example.com`:

```bash
curl -i https://api.itslanguage.nl -H "Origin: http://example.com"
```
```http
HTTP/1.1 302 Found
```

This is what the CORS preflight request looks like:

```bash
curl -i https://api.itslanguage.nl -H "Origin: http://example.com" -X OPTIONS
```
```http
HTTP/1.1 204 No Content
Access-Control-Allow-Origin: *
Access-Control-Allow-Headers: Content-Type
Access-Control-Allow-Methods: GET, POST, PUT, DELETE
Access-Control-Max-Age: 86400
Access-Control-Allow-Credentials: true
```
