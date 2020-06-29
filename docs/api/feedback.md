# Speech Feedback

## Create a feedback result

When a user has performed a speech recording and received feedback on it, the
feedback result can be stored using this call.
The feedback result is created for the current user.

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
sentences   | `json`   | **Optional** The feedback per sentence.
extra       | `json`   | **Optional** Extra data to store.
resultSetId | `string` | **Required** The unique id of the result set the recording belongs to. See [Recording result set](recordings.md#recording-result-set).

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
Content-Disposition: form-data; name="sentences"
Content-Type: application/json

{
  "sentence": 1,
  "errors": 1,
  "confidence": -159.0,
  "currentFrame": 64,
  "eosFrame": 61,
  "sessionId": "test",
  "tokenType": "EOS"
  "words": [
    {
      "sentenceIndex": 1,
      "textIndex": 1,
      "expected": "hello",
      "recognized": "hello",
      "label": "CW",
      "description": "Correct",
      "explanation": "The pronunciation matches the expected text.",
      "startTiming": 10,
      "endTiming": 400
    },
    {
      "sentenceIndex": 1,
      "textIndex": 2,
      "expected": "there",
      "recognized": "there-ERR",
      "label": "PC",
      "description": "Phonetic Change",
      "explanation": "One or more phones are changed.",
      "startTiming": 400,
      "endTiming": 620
    }
  ]
}
--YvHKkjjzXfysYJVHMoOAoNczae
Content-Disposition: form-data; name="sentences"
Content-Type: application/json

{
  "sentence": 2,
  "errors": 0,
  "confidence": -124.1,
  "currentFrame": 87,
  "eosFrame": 84,
  "sessionId": "test",
  "tokenType": "EOS"
  "words": [
    {
      "sentenceIndex": 2,
      "textIndex": 3,
      "expected": "general",
      "recognized": "general",
      "label": "CW",
      "description": "Correct",
      "explanation": "The pronunciation matches the expected text.",
      "startTiming": 40,
      "endTiming": 320
    },
    {
      "sentenceIndex": 2,
      "textIndex": 4,
      "expected": "kenobi",
      "recognized": "kenobi",
      "label": "CW",
      "description": "Correct",
      "explanation": "The pronunciation matches the expected text.",
      "startTiming": 320,
      "endTiming": 500
    }
  ]
}
--YvHKkjjzXfysYJVHMoOAoNczae
Content-Disposition: form-data; name="extra"
Content-Type: application/json

{'note': 'cheap microphone'}
--YvHKkjjzXfysYJVHMoOAoNczae
Content-Disposition: form-data; name="resultSetId"

5600160f578c4db6b7b0327e16e42153
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
  "sentences": [
    {
      "sentence": 1,
      "errors": 1,
      "confidence": -159.0,
      "currentFrame": 64,
      "eosFrame": 61,
      "sessionId": "test",
      "tokenType": "EOS"
      "words": [
        {
          "sentenceIndex": 1,
          "textIndex": 1,
          "expected": "hello",
          "recognized": "hello",
          "label": "CW",
          "description": "Correct",
          "explanation": "The pronunciation matches the expected text.",
          "startTiming": 10,
          "endTiming": 400
        },
        {
          "sentenceIndex": 1,
          "textIndex": 2,
          "expected": "there",
          "recognized": "there-ERR",
          "label": "PC",
          "description": "Phonetic Change",
          "explanation": "One or more phones are changed.",
          "startTiming": 400,
          "endTiming": 620
        }
      ]
    },
    {
      "sentence": 2,
      "errors": 0,
      "confidence": -124.1,
      "currentFrame": 87,
      "eosFrame": 84,
      "sessionId": "test",
      "tokenType": "EOS"
      "words": [
        {
          "sentenceIndex": 2,
          "textIndex": 3,
          "expected": "general",
          "recognized": "general",
          "label": "CW",
          "description": "Correct",
          "explanation": "The pronunciation matches the expected text.",
          "startTiming": 40,
          "endTiming": 320
        },
        {
          "sentenceIndex": 2,
          "textIndex": 4,
          "expected": "kenobi",
          "recognized": "kenobi",
          "label": "CW",
          "description": "Correct",
          "explanation": "The pronunciation matches the expected text.",
          "startTiming": 320,
          "endTiming": 500
        }
      ]
    }
  ],
  "extra": {
    "note": "cheap microphone"
  },
  "resultSet": {
      "id": "5600160f578c4db6b7b0327e16e42153",
      "combinedAudioUrl": "http://example.com/fullaudio.mp3",
    },
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
  "extra": {
    "note": "cheap microphone"
  },
  "resultSet": {
      "id": "5600160f578c4db6b7b0327e16e42153",
      "combinedAudioUrl": "http://example.com/fullaudio.mp3",
    },
  "userId": "24"
}
```
