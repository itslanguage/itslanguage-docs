# Speech Feedback

## Create a speech feedback

When a user has performed a speech recording and received feedback on it, the
feedback can be stored using this call.
The feedback is created for the current user.

### URL

```http
POST /challenges/speech/:challenge/feedback HTTP/1.1
```

* challenge - **Required** The id of the speech challenge used to perform the feedback.

### Request parameters

Name     | Type     | Description
---------|----------|------------
id       | `string` | **Optional** The id of the feedback. If none is given, one is generated.
audio    | `blob`   | **Required** The recorded audio.
feedback | `bool`   | **Required** The feedback as received per sentence.

### Request

The request should be a `multipart/form-data`.

Example:

```http
POST /challenges/speech/test/feedback HTTP/1.1
Accept: application/json
Content-Type: multipart/form-data; boundary="YvHKkjjzXfysYJVHMoOAoNczae"

--YvHKkjjzXfysYJVHMoOAoNczae
Content-Disposition: form-data; name="audio"; filename="blob"
Content-Type: audio/ogg

<audio blob>
--YvHKkjjzXfysYJVHMoOAoNczae
Content-Disposition: form-data; name="feedback"

True
--YvHKkjjzXfysYJVHMoOAoNczae
Content-Disposition: form-data; name="feedback"

False
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
  "audioUrl": "https://api.itslanguage.nl/download/YsjdG37bUGseu8-bsJ",
  "feedback": [
    true,
    false
  ],
  "userId": "24"
}
```
