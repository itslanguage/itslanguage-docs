# Speech Feedback

## Create a speech feedback

When a user has performed a speech recording and received feedback on it, the
feedback can be stored using this call.
The feedback is created for the current user.

### URL

```http
POST /feedback HTTP/1.1
```

### Request parameters

Name        | Type     | Description
------------|----------|------------
id          | `string` | **Optional** The id of the feedback. If none is given, one is generated.
audio       | `blob`   | **Required** The recorded audio.
challengeId | `string` | **Required** The id of the speech challenge used to perform the feedback.
errors      | `int`    | **Required** The amount of errors made per sentence.

### Request

The request should be a `multipart/form-data`.

Example:

```http
POST /feedback HTTP/1.1
Accept: application/json
Content-Type: multipart/form-data; boundary="YvHKkjjzXfysYJVHMoOAoNczae"

--YvHKkjjzXfysYJVHMoOAoNczae
Content-Disposition: form-data; name="challengeId"

chal
--YvHKkjjzXfysYJVHMoOAoNczae
Content-Disposition: form-data; name="audio"; filename="blob"
Content-Type: audio/ogg

<audio blob>
--YvHKkjjzXfysYJVHMoOAoNczae
Content-Disposition: form-data; name="errors"

0
--YvHKkjjzXfysYJVHMoOAoNczae
Content-Disposition: form-data; name="errors"

2
--YvHKkjjzXfysYJVHMoOAoNczae--
```

### Response

If the feedback is stored without error the stored feedback is returned as
JSON. Example:

```http
HTTP/1.1 201 Created
Content-Type: application/json

{
  "id": "34",
  "updated": "2018-02-20T09:39:40Z",
  "created": "2018-02-20T09:39:37Z",
  "audioUrl": "https://api.itslanguage.nl/feedback/34/audio",
  "challengeId": "chal",
  "errors": [
    0,
    1
  ],
  "userId": "24"
}
```


## Get feedback by id

A previously stored feedback can be fetched using a `GET` request.
Feedback can only be get by the owning user or by a user with the `TEACHER`
role.

### URL

```http
POST /feedback/:id HTTP/1.1
```

* id - **Required** The id of the feedback to get.

### Request

A `GET` request should be made, `application/json` should be accepted as a
valid return format. (All formats are accepted by default.)

```http
GET /feedback/34788437 HTTP/1.1
Accept: application/json
```

### Response

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": "34",
  "updated": "2018-02-20T09:39:40Z",
  "created": "2018-02-20T09:39:37Z",
  "audioUrl": "https://api.itslanguage.nl/feedback/34/audio",
  "challengeId": "chal",
  "errors": [
    0,
    2
  ],
  "userId": "24"
}
```
