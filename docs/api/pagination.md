# Pagination

Requests that return multiple items will be paginated to 20 items by
default.  You can set a custom page size up to 100 with the `?per_page` parameter.

```bash
curl "https://api.itslanguage.io/resources?per_page=100"
```

## Link Header

The pagination info is included in
[the Link header](http://tools.ietf.org/html/rfc5988). It is important to
follow these Link header values instead of constructing your own URLs. In some
instances, pagination is not based on page number.

```http
HTTP/1.1 200 OK
Link: <https://api.itslanguage.io/resources?page=for12c&per_page=100>; rel="next",
      <https://api.itslanguage.io/resources?page=back12c&per_page=100>; rel="prev"
```

_Linebreak is included for readability._

The possible `rel` values are:

Name    | Description
--------|------------
`next`  | The immediate next page of results.
`prev`  | The immediate previous page of results.
