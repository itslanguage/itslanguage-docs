## Filters

In some cases, when performing a GET, not all results are desirable. In
such a case a filter can be applied to return selective results. A filter can
be specified by using a query parameter. Multiple values for the same filter
can be given by specifying the filter multiple times.

For example, when listing entities using the URL `/entities` the result may be
as below:

```http
HTTP/1.1 200 OK
Content-Type: application/json

[
    {
        "id": "foo",
        "categories": ["a", "x"]
    },
    {
        "id": "bar",
        "categories": ["x"]
    }
]
```

If the API supports filtering on `category` (see the docs of the concerning
API), the results can be limited to a certain category by using a filter.
For example, to show only entities in category `a` the following request can
be made:

```http
GET /entities?category=a HTTP/1.1
```

When desired, a filter can be applied with multiple values. For example, to
show only entities in category `a` **and** category `x` the following request
can be made:

```http
GET /entities?category=a&category=x HTTP/1.1
```
