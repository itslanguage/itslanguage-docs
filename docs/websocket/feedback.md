# Speech feedback

It's possible to get feedback while recording. After every sentence feedback is
provided indicating whether or not the sentence was read well.
To perform a recording with feedback the following calls have to be made:

1. [Prepare the speech feedback](#prepare)
2. [Register audio procedure for streaming](wamp.md#register-audio-procedure)
3. [Start listening for audio](#listen)


## Prepare

Prepare a new speech feedback. This RPC should be called for each new speech
feedback. A unique id is generated for a speech feedback and a speech challenge
is prepared.

### URI

```
nl.itslanguage.feedback.prepare
```

### Parameters

Name            | Type     | Description
----------------|----------|------------
challenge_id    | `string` | **Required** The id of the speech challenge to prepare.

### Response

The id of the new speech recording is returned as a string.


## Start listening

In order to receive feedback the server needs to listen for audio on the
registered audio rpc. While listening the server will reply using progressive
results.
The server will stop listening when the audio rpc returns.

!!! note
    The server will only stop processing the audio when the audio rpc returns.

### URI

```
nl.itslanguage.feedback.listen_and_reply
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
  "feedback": true
}
```

Name     | Type   | Description
---------|--------|------------
sentence | `int`  | The index of the sentence, starting at `0`.
feedback | `bool` | Feedback score, `false` means bad and `true` means good.

When the recording is finished a recording with feedback is returned:

```json
{
  "id": "recording_1",
  "created": "2014-01-28T21:25:10Z",
  "updated": "2014-01-28T21:25:10Z",
  "audioUrl": "https://api.itslanguage.nl/download/audio.wav",
  "feedback": [
    false,
    true,
    true
  ]
}
```

Name     | Type     | Description
---------|----------|------------
id       | `string` | The id of the recording.
created  | `string` | The timestamp when the recording was created.
updated  | `string` | The timestamp when the recording was last updated.
audioUrl | `string` | The url to fetch the recorded audio.
feedback | `array`  | A list containing feedback (`bool`) per sentence.


## Pause

When desired the feedback recording can be paused. Doing so will stop the
server from processing the audio stream and returning feedback. Do note that
**the audio recording isn't paused when the feedback is paused**. To pause the
recording the audio RPC needs to stop sending data. For practical reasons it is
recommended to stop sending audio when the feedback is paused. Also see the
[Resume](#resume) docs for more info.

!!! note
    Pausing the feedback will not stop the feedback. See
    [the note on audio processing](#start-listening) how to stop it.

### URI

```
nl.itslanguage.feedback.pause
```

### Parameters

Name | Type     | Description
-----|----------|------------
id   | `string` | **Required** The id of the feedback to pause.


## Resume

A paused feedback can be resumed using this RPC. After this the audio stream is
again processed by the server.
Currently **it is required to re-send the audio header when resuming
feedback**. Failing to do so will fail the feedback as the server can't
recognise the audio format without a header.

!!! note
    It appears to be valid to have a wave file with multiple headers.

!!! warning
    It is required to re-send the audio header upon resume. The audio
    processing will fail if the header isn't re-sent.

### URI

```
nl.itslanguage.feedback.resume
```

### Parameters

Name        | Type     | Description
------------|----------|------------
id          | `string` | **Required** The id of the feedback to resume.
sentence_id | `int`    | **Required** The id of the sentence (starting with `0`) to resume on.
