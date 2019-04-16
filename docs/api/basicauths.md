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


## Delete a credential

### URL

To delete a basicauth for the current user:

```http
DELETE /user/basicauths/:basicauth HTTP/1.1
```

To delete a basicauth for another user, [impersonate the user](
oauth2.md#impersonation) first.

### Request

```http
DELETE /user/basicauths/1 HTTP/1.1
```

### Response

```http
HTTP/1.1 204 No Content
```
