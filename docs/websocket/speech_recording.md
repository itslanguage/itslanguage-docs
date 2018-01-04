# Speech Recording

Storing speech recordings is split up into multiple RPCs. Below, the various
RPCs are documented.

## Initialise a recording

Initialise a speech recording, this RPC should be called for each new speech
recording. A unique id is generated for a speech recording. This id is used
in the other RPCs to identify the recording,

### URI

```
nl.itslanguage.recording.init_recording
```

### Response

The id of the new speech recording is returned as a string.

## Initialising audio for uploading

The audio that is to be uploaded for analysis is streamed to the server. Some
information is required in order for the server to be able to store the audio
correctly.

### URI

```
nl.itslanguage.recording.init_audio
```

### Parameters

Name         | Type     | Description
-------------|----------|------------
recording_id | `string` | **Required** The unique id of the speech recording.
audio_format | `string` | **Required** The mimetype of the audio to upload.

Depending on the `audio_format`, additional parameters may be required.
Please see [the audio documentation](audio.md) for which audio formats are
supported and what parameters they require.


## Initialise recording challenge

A challenge should be linked when storing the recording. This call verifies
asynchronously that the challenge exists. If the challenge or the organisation
doesn't exist, an error is raised. Since this happens asynchronously, the error
is raised later.

### URI

```
nl.itslanguage.recording.init_challenge
```

### Parameters

Name            | Type     | Description
----------------|----------|------------
recording_id    | `string` | **Required** The unique id of the speech recording.
organisation_id | `string` | **Required** The id of the organisation in which to find the challenge.
challenge_id    | `string` | **Required** The id of the challenge the recording is meant for.

## Stream recording audio

The recorded audio can now be streamed to the server. The streaming works by
repeatedly calling this RPC. Each time the RPC is called, the data
will be appended to an audio file on the server.

### URI

```
nl.itslanguage.recording.write
```

### Parameters

Name         | Type     | Description
-------------|----------|------------
recording_id | `string` | **Required** The unique id of the speech recording.
data         | `bytes`  | **Required** The data to stream to the server. This can be any length.
encoding     | `string` | **Optional** The encoding in which the data is sent. This should be `base64` if the data is base64 encoded.


## Close recording

Close the recording and store it on the server. After this call, no data can be
written to the recording any more.

### URI

```
nl.itslanguage.recording.close
```

### Parameters

Name         | Type     | Description
-------------|----------|------------
recording_id | `string` | **Required** The unique id of the speech recording.

The RPC can return various errors:

* `nl.itslanguage.no_such_organisation` If the organisation doesn't exist.
* `nl.itslanguage.no_such_challenge` If the challenge doesn't exist.


## Feedback

It's possible to get feedback while recording. After every sentence feedback is
provided indicating whether or not the sentence was read well.

### URI

```
nl.itslanguage.recording.feedback
```

### Parameters

Name         | Type     | Description
-------------|----------|------------
recording_id | `string` | **Required** The unique id of the speech recording.
rpc          | `string` | **Required** The URI of a [registered audio rpc](wamp.md#register-audio-procedure).

### Response

The rpc returns progressive results for realtime feadback. After every sentence
the following json is sent as a progressive result:

```json
{
  "sentence": 0,
  "feedback": 1
}
```

Name     | Type  | Description
---------|-------|------------
sentence | `int` | The index of the sentence, starting at `0`.
feedback | `int` | Feedback score, `0` means bad and `1` means good.

When the recording is finished a recording with feedback is returned:

```json
{
  "id": "recording_1",
  "created": "2014-01-28T21:25:10Z",
  "updated": "2014-01-28T21:25:10Z",
  "audioUrl": "https://api.itslanguage.nl/download/audio.wav",
  "feedback": [
    0,
    1,
    1
  ]
}
```

Name     | Type     | Description
---------|----------|------------
id       | `string` | The id of the recording.
created  | `string` | The timestamp when the recording was created.
updated  | `string` | The timestamp when the recording was last updated.
audioUrl | `string` | The url to fetch the recorded audio.
feedback | `array`  | A list containing feedback (`int`) per sentence.
