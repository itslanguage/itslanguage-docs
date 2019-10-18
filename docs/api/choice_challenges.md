# Choice Challenges

A set of transcription choices is used to perform speech recognition to
select one of the choices.


## Fields

A choice challenge contains the following fields. It is also noted whether it
is a required or optional parameter.

Name       | Type       | Description
-----------|------------|------------
`id`       | `string`   | **Optional** The choice challenge identifier. If none is given, one is generated.
`question` | `string`   | **Optional** A hint or question related to the choices.
`choices`  | `string[]` | **Required** The sentences of which at most one may be recognised.
`audio`    | `blob[]`   | **Optional** Audible sentences that match the ones in `choices` at the same index.
`status`   | `string`   | **Required** The status of the challenge's preparation. Either 'unprepared', 'preparing' or 'prepared'. After creating a choice challenge, the status is 'preparing' for a short while, after it'll become 'prepared' to indicate the challenge is ready to perform a speech recognition on, or it'll be 'unprepared' in case the preparation failed for any reason.
`language` | `string`   | **Required** The language of the challenge in ISO 639-2 format

## List all choice challenges

### URL

```http
GET /challenges/choice HTTP/1.1
```

### Request

```http
GET /challenges/choice HTTP/1.1
Accept: application/json
```

### Response

The response is a JSON list with choice challenges.

The second entry shows that _choice challenges_ can also be used to recognise
one specific sentence.

```http
HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "id": "13",
    "created": "2014-01-28T21:25:10Z",
    "updated": "2014-01-28T21:25:10Z",
    "question": "The Pitcairn Islands are ... in the South Pacific Ocean",
    "choices": [
      {
        "choice": "located",
        "audioUrl": "https://api.itslanguage.io/download/YsjdG37bUGseu8-bsJ"
      }, {
        "choice": "sited",
        "audioUrl": null
      }, {
        "choice": "stationed",
        "audioUrl": null
      }, {
        "choice": "settled",
        "audioUrl": null
      }
    ],
    "status": "prepared",
    "language": "eng"
  },
  {
    "id": "14",
    "created": "2014-01-28T21:25:10Z",
    "updated": "2014-01-28T21:25:10Z",
    "question": "Ik heb erge hoofdpijn.",
    "choices": [
      {
        "choice": "I have a very bad headache.",
        "audioUrl": null
      }
    ],
    "status": "preparing",
    "language": "eng"
  }
]
```

## Get a single choice challenge

### URL

```http
GET /challenges/choice/:challenge HTTP/1.1
```

* `challenge` - **Required** The choice challenge identifier.

### Request

```http
GET /challenges/choice/choice_1 HTTP/1.1
Accept: application/json
```

### Response

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": "13",
  "created": "2014-01-28T21:25:10Z",
  "updated": "2014-01-28T21:25:10Z",
  "question": "The Pitcairn Islands are ... in the South Pacific Ocean",
  "choices": [
    {
      "choice": "located",
      "audioUrl": "https://api.itslanguage.io/download/YsjdG37bUGseu8-bsJ"
    }, {
      "choice": "sited",
      "audioUrl": null
    }, {
      "choice": "stationed",
      "audioUrl": null
    }, {
      "choice": "settled",
      "audioUrl": null
    }
  ],
  "status": "prepared",
  "language": "eng"
}
```


## Create a choice challenge

### URL

```http
POST /challenges/choice HTTP/1.1
```

### Request

```http
POST /challenges/choice HTTP/1.1
Accept: application/json
Content-Type: multipart/form-data; boundary=jhgd87g7Gy3d78

--jhgd87g7Gy3d78
Content-Disposition: form-data; name="choices"

located
--jhgd87g7Gy3d78
Content-Disposition: form-data; name="choices"

sited
--jhgd87g7Gy3d78
Content-Disposition: form-data; name="audio"; filename="blob"
Content-Type: audio/ogg

<spoken audio: located>
--jhgd87g7Gy3d78
Content-Disposition: form-data; name="audio"; filename="blob"
Content-Type: audio/wav

<spoken audio: sited>
--jhgd87g7Gy3d78--
```

### Response

```http
HTTP/1.1 202 Accepted
Content-Type: application/json
Location: https://api.itslanguage.io/challenges/choice/4

{
  "id": "4",
  "created": "2014-01-28T21:25:10Z",
  "updated": "2014-01-28T21:25:10Z",
  "question": "The Pitcairn Islands are ... in the South Pacific Ocean",
  "choices": [
    {
      "choice": "located",
      "audioUrl": "https://api.itslanguage.io/download/YsjdG37bUGseu8-bsJ"
    }, {
      "choice": "sited",
      "audioUrl": "https://api.itslanguage.io/download/YsjdG37bUGseu8-bsK"
    }
  ],
  "status": "preparing",
  "language", "eng"
}
```


## Update a choice challenge

Update one or more properties of an existing choice challenge.

### URL

```http
PUT /challenges/choice/:challenge HTTP/1.1
```

* `challenge` - **Required** The choice challenge identifier.

### Request Parameters

See [Fields](#fields) for parameter names. All parameters are optional as this
concerns an incremental update.

### Request

```http
PUT /challenges/choice/4 HTTP/1.1
Accept: application/json
Content-Type: application/json

{
  "choices": [
    "located",
    "sited",
    "stationed",
    "settled"
  ]
}
```

Or, with a preference for a web form submit:

```http
PUT /challenges/choice HTTP/1.1
Content-Type: application/x-www-form-urlencoded

choices=located&choices=sited&choices=stationed&choices=settled
```

### Response fields

For all fields see [Fields](#fields).
Noteworthy things are below.

Name     | Note
---------|------------
`status` | Once a choice challenge has been updated, no speech recognitions can be performed on it until the status becomes 'prepared' again, even though the choice challenge was 'prepared' before the update.

### Response

```http
HTTP/1.1 200 OK
Content-Type: application/json
Location: https://api.itslanguage.io/challenges/choice/4

{
  "id": "4",
  "created": "2014-01-28T21:25:10Z",
  "updated": "2014-01-28T21:25:10Z",
  "question": "The Pitcairn Islands are ... in the South Pacific Ocean",
  "choices": [
    "located",
    "sited",
    "stationed",
    "settled"
  ],
  "status": "preparing",
  "language": "eng"
}
```

## Delete a single choice challenge

### URL

```http
DELETE /challenges/choice/:challenge HTTP/1.1
```

* `challenge` - **Required** The choice challenge identifier.

### Request

```http
DELETE /challenges/choice/choice_1 HTTP/1.1
```

### Response

```http
HTTP/1.1 204 No Content
```
