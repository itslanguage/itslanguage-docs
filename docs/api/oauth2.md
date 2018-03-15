# OAuth2

Access tokens may be requested by users to grant them access.

An access token can be used to make an authorised request. To do so, the
access token should be passed as an [authorization request
header](#using-the-access-token).

## Obtain access token

An access token can either be requested using a manual request or by using an
OAuth2 compatible client.

### Request

To request an access token, a POST request should be done to the access token
API. The body should contain the username, password, grant type (`password`)
and optional scope. To request an access token for the user `tenant` with
password `secret` and scope `tenant/tenant`, the request should look like this:

```http
POST https://api.itslanguage.nl/tokens HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Accept: application/json

grant_type=password&username=tenant&password=secret&scope=tenant%2Ftenant
```

!!! note
    The scope is best omitted by Murphy's law.

!!! note
    The scope **is required** when authenticating as a user in an organisation,
    using a username and password. This is because a user is only unique within
    the organisation and there is no way of determining the organisation from
    the username alone. For a user with the username `bob` in the organisation
    `org` in tenant `ten` the scope should be
    `tenant/ten/organisation/org/user/bob`.
    When using email authentication, passing the scope is not neccesary.

!!! note
    The `Content-Type` of the request is different from the response.

### Response

When the supplied credentials are valid, an access token is returned as a JSON
response:

```http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache

{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJI",
    "token_type": "Bearer",
    "scope": "tenant/tenant"
}
```

## Using the access token

The access token can be used to make an authorised request. To do so, the
access token should be placed in the Authorization header as a bearer token.
Example:

```http
GET https://api.itslanguage.nl/organisations HTTP/1.1
Accept: application/json
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJI
```

In cases where it is not possible to use the `Authorization` header, the token
can be supplied using a request parameter:

```http
GET https://api.itslanguage.nl/organisations?access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJI HTTP/1.1
```

!!! note
    The example access tokens are shortened for demo purposes.

## Impersonation

It is possible to impersonate another user by using the `client_credentials`
grant type and supplying a scope that represents the impersonation target.
This can be convenient for a variety of reasons.

When a tenant wants to perform authentication for its users instead of using
the built in authentication, the tenant can use its credentials to request
tokens for its users. The token can be relayed to the end-user so the tenant
actually authenticated and impersonated the user.

When a top level admin user wants to perform administrative tasks for a
tenant/organisation, the admin can impersonate the appropriate
tenant/organisation.

Basically impersonation is allowed top-down; a tenant can impersonate an
organisation or user but a user can't impersonate any of them.
Depending on the user's privileges a user may or may not impersonate. (A
tenant admin user may impersonate the same things a tenant can for example.)

### Example scopes

Scope                                      | Description
-------------------------------------------|------------
`user/barry`                               | Top level admin user with id `barry`
`tenant/ten`                               | Tenant with id `ten`
`tenant/demo/organisation/org`             | Organisation `org` in tenant `demo`
`tenant/demo/organisation/org/student/stu` | Student `stu` in organisation `org` in tenant `demo`

### Request

To request an access token a POST request should be done to the access token
API. The body (`application/x-www-form-urlencoded`) should contain the target
scope and the grant type (`client_credentials`). The request should also
contain a previously received access token in the `Authorization` header as
described in [Using the access token](#using-the-access-token).
Example request:

```http
POST https://api.itslanguage.nl/tokens HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Accept: application/json
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJI

grant_type=client_credentials&scope=tenant%2Ftenant

```

If the authentication succeeds the access token is returned as [usual](#response)

!!! note
    If no scope is supplied (which is valid in the OAuth2 spec), an access
    token for the current user is returned (meaning no impersonation takes
    place).
