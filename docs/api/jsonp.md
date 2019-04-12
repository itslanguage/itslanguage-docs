## JSON-P Callbacks
You can send a `?callback` parameter to any GET call to have the results
wrapped in a JavaScript function.  This is typically used when browsers want
to embed content in web pages by getting around cross domain
issues.  The response includes the same data output as the regular API,
plus the relevant HTTP Header information.

```bash
curl "https://api.itslanguage.nl?callback=foo"
```
```js
foo({
  "meta": {
    "status": 200,
    "Link": [ // pagination headers and other links
      ["https://api.itslanguage.nl?cursor=next2", {"rel": "next"}]
    ]
  },
  "data": {
    // the data
  }
})
```

You can write a JavaScript handler to process the callback like this:

```js
function foo(response) {
  var meta = response.meta
  var data = response.data
  console.log(meta)
  console.log(data)
}
```

All of the headers are the same String value as the HTTP Headers with one
notable exception: Link.  Link headers are pre-parsed for you and come
through as an array of `[url, options]` tuples.
