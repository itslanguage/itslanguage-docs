# Roles

Roles are named groups of permissions. A role is typically assigned to a user
to grant a user permissions.

## Create a role

### URL

```http
POST /roles/role HTTP/1.1
```

### Request parameters

Name               | Type      | Description
-------------------|-----------|------------
`id`               | `string`  | **Optional** The category identifier. If none is given, one is generated.
`permissions`      | `array`   | **Required** Array of permissions this role is authorized for.

### Request

```http
POST /users HTTP/1.1
Accept: application/json
Content-Type: application/json

{
  "id": "Student",
  "permissions": [
       "CHOICE_CHALLENGE_CREATE",
       "CHOICE_CHALLENGE_DELETE"
  ]
}
```


### Response

```http
HTTP/1.1 201 Created
Content-Type: application/json
Location: https://api.itslanguage.io/roles/Student

{
  "id": "Student",
  "permissions": [
       "CHOICE_CHALLENGE_CREATE",
       "CHOICE_CHALLENGE_DELETE"
  ]
}
```

## Get a single role

### URL

```http
GET /roles/:role HTTP/1.1
```

* `role` - **Required** The role identifier.

### Request

```http
GET /roles/Student HTTP/1.1
Accept: application/json
```

### Response

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": "Student",
  "permissions": [
        "CHOICE_CHALLENGE_CREATE",
        "CHOICE_CHALLENGE_DELETE"
    ]
}
```

## List roles

### URL

```http
GET /roles HTTP/1.1
```

### Request

```http
GET /roles HTTP/1.1
Accept: application/json
```

### Response

```http
HTTP/1.1 201 Created
Content-Type: application/json

[
  {
    "name": "Admin",
    "permissions": [
        "CHOICE_CHALLENGE_CREATE",
        "CHOICE_CHALLENGE_DELETE"
    ]
  },
  {
    "name": "Student",
    "permissions": [
        "CHOICE_CHALLENGE_List"
    ]
  }
]
```
