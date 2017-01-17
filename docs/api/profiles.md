# Profiles

Profiles contain information about a User. A User does not need a Profile, but a Profile is always created
together with a User.

## Get the profile of a user

### URL

```http
GET /users/:userId/profile HTTP/1.1
```

### Request

```http
GET /users/user_23/profile HTTP/1.1
Accept: application/json
```

### Response

```http
HTTP/1.1 201 Created
Content-Type: application/json

{
  "id": "profile_23",
  "firstName": "Juriaan",
  "infix": null,
  "lastName": "Winkens",
  "gender": "Male",
  "birthDate":"1996-16-07T00:00:00Z",
  "created": "2017-01-03T12:16:55Z",
  "updated": "2017-01-03T12:16:55Z"
}
```

## Get the profile of the current user

### URL

```http
GET /user/profile/ HTTP/1.1
```

### Request

```http
GET /user/profile/ HTTP/1.1
Accept: application/json
```

### Response

```http
HTTP/1.1 201 Created
Content-Type: application/json

{
  "id": "profile_23",
  "firstName": "Juriaan",
  "infix": null,
  "lastName": "Winkens",
  "gender": "Male",
  "birthDate":"1996-16-07T00:00:00Z",
  "created": "2017-01-03T12:16:55Z",
  "updated": "2017-01-03T12:16:55Z"
}
```

## List profiles

List all profiles in the active organisation.

### URL

```http
GET /profiles HTTP/1.1
```

### Request

```http
GET /profiles HTTP/1.1
Accept: application/json
```

### Response

```http
HTTP/1.1 200 OK
Content-Type: application/json

[
  {
      "id": "profile_23",
      "firstName": "Juriaan",
      "infix": null,
      "lastName": "Winkens",
      "gender": "Male",
      "birthDate":"1996-16-07T00:00:00Z",
      "created": "2017-01-03T12:16:55Z",
      "updated": "2017-01-03T12:16:55Z"
  },
  {
      "id": "profile_24",
      "firstName": "Loek",
      "infix": null,
      "lastName": "Ehren",
      "gender": "Male",
      "birthDate":"1995-12-03T00:00:00Z",
      "created": "2017-01-03T12:16:55Z",
      "updated": "2017-01-03T12:16:55Z"
  }
]
```


