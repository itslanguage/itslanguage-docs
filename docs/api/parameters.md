# Parameters

Many API methods take optional parameters. For GET requests, any parameters not
specified as a segment in the path can be passed as an HTTP query string
parameter:

```bash
curl -i "https://api.itslanguage.nl/organisations/sheffield/students/?first_name=mauro"
```

In this example, the 'sheffield' value is provided for the `:organisation`
parameter in the path while `:first_name` is passed in the query
string.

For POST, PUT, and DELETE requests, parameters not included in the URL should
be encoded as form data with a Content-Type of 'application/x-www-form-urlencoded':

```bash
curl -i -d 'first_name=Mauro' https://api.itslanguage.nl/organisations/sheffield/students
```

Or the parameters should be encoded as JSON with a Content-Type of 'application/json':

```bash
curl -i -d '{"firstName": "Mauro"}' -H 'application/json' https://api.itslanguage.nl/organisations/sheffield/students
```
