# Parameters

Some API methods take optional parameters. For GET requests, any parameters not
specified as a segment in the path can be passed as an HTTP query string
parameter.

For example when listing entities using the url `/entities` with some `field`:

```bash
curl -i "https://api.itslanguage.io/entitites/entity?field=magic"
```

In this example, the 'entity' value is provided for the `:entity` parameter in
the path while `:field` is passed in the query string.

For POST, PUT, and DELETE requests, parameters not included in the URL should
be encoded as form data with a Content-Type of 'application/x-www-form-urlencoded':

```bash
curl -i -d 'field=magic' https://api.itslanguage.io/entities/entity
```

Or the parameters should be encoded as JSON with a Content-Type of 'application/json':

```bash
curl -i -d '{"field": "value"}' -H 'application/json' https://api.itslanguage.io/entities/entity
```
