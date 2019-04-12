# Data representation

## Timestamps

All timestamps are returned in ISO 8601 format `YYYY-MM-DDTHH:MM:SSZ`.

## Summary Representations

When fetching a list of resources, the response includes a _subset_ of the
attributes for that resource. This is the "summary" representation of the
resource. (Some attributes are computationally expensive for the API to provide.
For performance reasons, the summary representation excludes those attributes.
To obtain those attributes, fetch the "detailed" representation using a
dedicated API.)

## Detailed Representations

When fetching individual resources, a response typically includes _all_
attributes for that resource. This is the "detailed" representation of the
resource. (Note that authorization sometimes influences the amount of detail
included in the representation.)

The documentation provides an example response for each API method. The example
response illustrates all attributes that are returned by that method.
