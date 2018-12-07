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
birthYear    | `int`      | **Optional** The user's year of birth.
gender       | `string`   | **Optional** The user's gender ("Male"|"Female").
country      | `string`   | **Optional** The country of the user's mother tongue.
language     | `string`   | **Optional** The language of the user's mother tongue. In ISO 639-2 format.
cefr         | `string`   | **Optional** The user's CEFR (Common European Framework of Reference) level.

### Request

```http
POST /users HTTP/1.1
Accept: application/json
Content-Type: application/json

{
  "birthYear": 1923,
  "gender": "Male",
  "country": "Netherlands",
  "language": "nld",
  "cefr": "A2"
}
```

### Response

```http
HTTP/1.1 201 Created
Content-Type: application/json

{
  "birthYear": 1923,
  "gender": "Male",
  "country": "Netherlands",
  "language": "nld",
  "cefr": "A2",
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
HTTP/1.1 200 Created
Content-Type: application/json

{
  "birthYear": 1923,
  "gender": "Male",
  "country": "Netherlands",
  "language": "nld",
  "cefr": "A2",
  "created": "2017-01-03T12:16:55Z",
  "updated": "2017-01-03T12:16:55Z"
}
```

## Get the profile of the current user

### URL

```http
GET /user/profile HTTP/1.1
```

### Request

```http
GET /user/profile HTTP/1.1
Accept: application/json
```

### Response

```http
HTTP/1.1 200 Created
Content-Type: application/json

{
  "birthYear": 1923,
  "gender": "Male",
  "country": "Netherlands",
  "language": "nld",
  "cefr": "A2",
  "created": "2017-01-03T12:16:55Z",
  "updated": "2017-01-03T12:16:55Z"
}
```



