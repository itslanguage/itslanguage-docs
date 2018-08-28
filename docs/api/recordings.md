# Recordings

Audio can be stored using the ITSLanguage Speech API.
It can optionally be retrieved for processing or playback at a later time.

Type     | Usage
---------|------
`speech` | Record user submitted audio for a speech challenge.

## List all speech recordings

### URL

```http
GET /challenges/speech/:challenge/recordings HTTP/1.1
```

* challenge - **Required** The challenge identifier.

### Request

```http
GET /challenges/speech/test/recordings HTTP/1.1
Accept: application/json
```

### Response

The response is a JSON list with speech recordings.

```http
HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "id": "recording_1",
    "created": "2014-01-28T21:25:10Z",
    "updated": "2014-01-28T21:25:10Z",
    "audioUrl": "https://api.itslanguage.nl/download/YsjdG37bUGseu8-bsJ",
    "userId": "24"
  },
  {
    "id": "14",
    "created": "2014-01-28T21:25:10Z",
    "updated": "2014-01-28T21:25:10Z",
    "audioUrl": "https://api.itslanguage.nl/download/YsjdG37bUGseu8-Xaq",
    "userId": "24"
  }
]
```


## Get a single speech recording

### URL

```http
GET /challenges/speech/:challenge/recordings/:recording HTTP/1.1
```

* challenge - **Required** The challenge identifier.
* recording - **Required** The recording identifier.

### Request

```http
GET /challenges/speech/speech_1/recordings/recording_1 HTTP/1.1
Accept: application/json
```

### Response

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": "speech_1",
  "created": "2014-01-28T21:25:10Z",
  "updated": "2014-01-28T21:25:10Z",
  "audioUrl": "https://api.itslanguage.nl/download/YsjdG37bUGseu8-bsJ",
  "userId": "24"
}
```


## Create a speech recording

Submit a new audio recording to persist without immediate analysis.
The recording is created as the current user.

### URL

```http
POST /challenges/speech/:challenge/recordings HTTP/1.1
```

* challenge - **Required** The challenge identifier.

### Request parameters

Name        | Type     | Description
------------|----------|------------
id          | `string` | **Optional** The recording identifier. If none is given, one is generated.
audio       | `blob`   | **Required** The actual audio. (Ogg Opus or WAV)
resultSetId | `string` | **Required** The unique id of the result set the recording belongs to. See [Recording result set](#recording-result-set).

### Request

The request should be a `multipart/form-data`.

```http
POST /challenges/speech/test/recordings HTTP/1.1
Accept: application/json
Content-Type: multipart/form-data; boundary="YvHKkjjzXfysYJVHMoOAoNczae"

--YvHKkjjzXfysYJVHMoOAoNczae
Content-Disposition: form-data; name="audio"; filename="blob"
Content-Type: audio/ogg

<audio blob>
--YvHKkjjzXfysYJVHMoOAoNczae--
```

### Response

If the audio was persisted correctly:

```http
HTTP/1.1 201 Created
Content-Type: application/json

{
  "id": "34",
  "updated": "2014-02-13T09:39:40Z",
  "created": "2014-02-13T09:39:37Z",
  "audioUrl": "https://api.itslanguage.nl/download/YsjdG37bUGseu8-bsJ",
  "userId": "24"
}
```


# Recording result set

In order to group recordings together a result set is used. The result set
contains multiple recordings and an audio file containing all the audio files
of its recordings.

A result set is created automatically whenever a recording is created with
a non-existing `resultSetId`. When the `resultSetId` does exist the recording
is added to this set and the audio of the recording is appended to the audio
of the result set.

The result set cannot be queried directly but it can be seen when getting
[progress](progress.md).
