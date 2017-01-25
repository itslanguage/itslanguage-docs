# Groups

Users can be part of zero or multiple groups.

## Create a group

### URL

```http
POST /groups HTTP/1.1
```

### Input

Name         | Type       | Description
-------------|------------|------------
id           | `string`   | **Optional** A unique identifier. If none is given, one is generated.
name         | `string`   | **Required** The name of the group.

### Request

```http
POST /groups HTTP/1.1
Accept: application/json
Content-Type: application/json

{
  "id": "GROUP_1",
  "name": "First Grade"
}
```

### Response

```http
HTTP/1.1 201 Created
Content-Type: application/json

{
  "id": "GROUP_1",
  "name": "First Grade",
  "created": "2016-12-22T02:14:05Z",
  "updated": "2016-12-22T02:14:05Z"
}
```


## List groups

### URL

```http
GET /groups HTTP/1.1
```

### Request

```http
GET /groups HTTP/1.1
Accept: application/json
```

### Response

```http
HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "id": "GROUP_1",
    "name": "First Grade",
    "created": "2016-12-22T02:14:05Z",
    "updated": "2016-12-22T02:14:05Z"
  },
  {
    "id": "GROUP_2",
    "name": "Second Grade",
    "created": "2017-01-01T14:05:05Z",
    "updated": "2017-01-01T14:05:05Z"
  }
]
```


## Get a single group

### URL

```http
GET /groups/:group HTTP/1.1
```

* group - **Required** The identifier of the group.

### Request

```http
GET /groups/GROUP_1 HTTP/1.1
Accept: application/json
```

### Response

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": "GROUP_1",
  "name": "First Grade",
  "created": "2000-04-17T19:20:23Z",
  "updated": "2001-12-14T07:38:45Z"
}
```
