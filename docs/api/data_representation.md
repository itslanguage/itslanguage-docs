# Data representation

## Timestamps

All timestamps are returned in ISO 8601 format `YYYY-MM-DDTHH:MM:SSZ`.

## Summary Representations

When you fetch a list of resources, the response includes a _subset_ of the
attributes for that resource. This is the "summary" representation of the
resource. (Some attributes are computationally expensive for the API to provide.
For performance reasons, the summary representation excludes those attributes.
To obtain those attributes, fetch the "detailed" representation.)

**Example**: When you get a list of students, you get the summary
representation of each student. Here, we fetch the list of students owned
by the [sheffield]() organisation:

```http
GET /organisations/sheffield/students HTTP/1.1
```

## Detailed Representations

When you fetch an individual resource, the response typically includes _all_
attributes for that resource. This is the "detailed" representation of the
resource. (Note that authorization sometimes influences the amount of detail
included in the representation.)

**Example**: When you get an individual repository, you get the detailed
representation of the repository. Here, we fetch the
[sheffield/amy]() student:

```http
GET /organisations/sheffield/students/amy HTTP/1.1
```

The documentation provides an example response for each API method. The example
response illustrates all attributes that are returned by that method.
