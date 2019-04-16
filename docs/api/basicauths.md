# Username/password authentication

Users can have username/password based credentials (basicauth). These
credentials can be managed using the REST API. Only users with administrative
powers can perform these calls.


## Create credentials

### URL

To create a basicauth for the current user:

```http
POST /user/basicauths HTTP/1.1
```

To create a basicauth for another user, [impersonate the user](
oauth2.md#impersonation) first.

When creating a basicauth for a user in an organisation, the username doesn't
need to be unique across organisations. The username **does need to be unique**
within the organisation.

### Input

Name     | Type     | Description
---------|----------|------------
username | `string` | **Required** A unique username.
password | `string` | **Required** A secure password.

### Request

```http
POST /user/basicauths HTTP/1.1
Accept: application/json
Content-Type: application/json

{
  "username": "harry",
  "password": "alohomora"
}
```

### Response

```http
HTTP/1.1 201 Created
Content-Type: application/json

{
    "email": "harry",
    "password": "alohomora"
}
```


## Update a credential

### URL

To update a basicauth for the current user:

```http
PUT /user/basicauths/:basicauth HTTP/1.1
```

To update a basicauth for another user, [impersonate the user](
oauth2.md#impersonation) first.

When updating a basicauth for a user in an organisation, the username doesn't
need to be unique across organisations. The username **does need to be unique**
within the organisation.

### Input

Leave either parameter empty to keep the current value.

Name     | Type     | Description
---------|----------|------------
username | `string` | **Optional** A new unique username.
password | `string` | **Optional** A new secure password.

### Request

```http
PUT /user/basicauths/1 HTTP/1.1
Accept: application/json
Content-Type: application/json

{
  "username": "harry",
  "password": "alohomora"
}
```

### Response

```http
HTTP/1.1 200 Ok
Content-Type: application/json

{
    "email": "harry",
    "password": "alohomora"
}
```
