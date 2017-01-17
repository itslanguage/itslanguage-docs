# Profiles

Profiles contain information about a User. A User does not need a Profile.

## Create a profile

Link a profile to a user. If a user already has a profile, it will be overwritten.

### URL

```http
POST /users/:userId/profile HTTP/1.1
```

### Input

Name         | Type       | Description
-------------|------------|------------
id           | `string`   | **Optional** A unique identifier. If none is given, one is generated.
firstName    | `object`   | **Optional** The profile containing information about the user.
lastName     | `string`   | **Optional** The groups this user is part of.
infix        | `string`   | **Optional** The names of roles to grant the user.
gender       | `string`   | **Optional** The gender of the user.
birthDate    | `string`   | **Optional** The birthDate of the user. In the proper ISO 8601 format.

### Request

```http
POST /users HTTP/1.1
Accept: application/json
Content-Type: application/json

{
  "firstName": "Najat",
  "infix": "van der",
  "lastName": "Lee",
  "gender": "Male",
  "birthDate": "1996-16-07T00:00:00Z"
}
```

### Response

```http
HTTP/1.1 201 Created
Content-Type: application/json

{
  "id": "sdcjb823jhguys5j",
  "firstName": "Najat",
  "infix": "van der",
  "lastName": "Lee",
  "gender": "Male",
  "birthDate": "1996-16-07T00:00:00Z",
  "created": "2016-12-22T02:14:05Z",
  "updated": "2016-12-22T02:14:05Z"
}
```

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


