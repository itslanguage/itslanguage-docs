# Speech feedback

It's possible to get feedback while recording. After every sentence feedback is
provided indicating whether or not the sentence was read well.
To perform a recording with feedback the following calls have to be made:

1. [Meet prerequisites](#prerequisites)
2. [Prepare the speech feedback](#prepare)
3. [Register audio procedure for streaming](wamp.md#register-audio-procedure)
4. [Start listening for audio](#listen)

## Prerequisites

To be able to do speech feedback the following prerequisites needs to be met:

- A speech challenge must be used so that needs to exist.
- The speech challenges needs to have the language set
- The user performing the speech feedback needs to have a valid profile with
  birthYear and language set.

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
  "feedback_id": "recording_1",
  "sentence": 0,
  "errors": 1,
  "confidence": -78.0,
  "currentFrame": 68,
  "eosFrame": 63,
  "sessionId": "test",
  "tokenType": "EOS"
  "words": [
    {
      "sentenceIndex": 0,
      "textIndex": 1,
      "expected": "funny",
      "recognized": "funny",
      "label": "CW",
      "description": "Correct.",
      "explanation": "The pronunciation matches the expected text."
    },
    {
      "sentenceIndex": 0,
      "textIndex": 1,
      "expected": "gif",
      "recognized": "gif-ERR",
      "label": "PC",
      "description": "Phonetic Change",
      "explanation": "One or more phones are changed."
    }
  ]
}
```

Name         | Type     | Description
-------------|----------|------------
feedback_id  | `string` | The unique id of the feedback this results belongs to.
sentence     | `int`    | The index of the sentence, starting at `0`.
errors       | `int`    | Amount of errors made.
confidence   | `float`  | Confidence of the end of sentence detection.
currentFrame | `int`    | Audio frame of end of sentence detection.
eosFrame     | `int`    | Audio frame of the end of sentence.
sessionId    | `string` | Session identifier.
tokenType    | `string` | The detected token.
words        | `list`   | All expected and recognized words in the sentence.

The list of words each contain the following fields:

Name          | Type     | Description
--------------|----------|------------
sentenceIndex | `int`    | Index of the sentence.
textIndex     | `int`    | Index of the whole text.
expected      | `string` | The word as it was expected to be pronounced.
recognized    | `string` | The recognized result. If nothing is recognized this field is `null`.
label         | `string` | Label describing what was recognized.
description   | `string` | Description of the label.
explanation   | `string` | in-depth explanation of the label.
startTiming   | `int`    | Beginning of the section in milliseconds.
endTiming     | `int`    | End of a section in milliseconds.

Other than a correct word labels can indicate errors in the pronunciation. These
are all the possible labels that can currently be assigned to a word.

Label | Description     | Explanation
------|-----------------|------------
`CW`  | Correct         | The pronunction matches the expected text.
`SL`  | Silence         | Anomalous silence in-between words.
`RW`  | Repetition      | Expected word was repeated.
`OW`  | Omission        | Expected word was omitted.
`PC`  | Phonetic Change | One or more phones are changed.
`FS`  | False Start     | Partial in-prompt realisation.

When the recording is finished a recording with feedback is returned:

```json
{
  "id": "recording_1",
  "created": "2014-01-28T21:25:10Z",
  "updated": "2014-01-28T21:25:10Z",
  "audioUrl": "https://api.itslanguage.nl/download/audio.wav",
  "sentences": [
    {
      "sentence": 0,
      "errors": 1,
      "confidence": -159.0,
      "currentFrame": 64,
      "eosFrame": 61,
      "sessionId": "test",
      "tokenType": "EOS"
      "words": [
        {
          "sentenceIndex": 0,
          "textIndex": 0,
          "expected": "hello",
          "recognized": "hello",
          "label": "CW",
          "description": "Correct",
          "explanation": "The pronunciation matches the expected text.",
          "startTiming": 10,
          "endTiming": 400
        },
        {
          "sentenceIndex": 0,
          "textIndex": 1,
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
      "sentence": 1,
      "errors": 0,
      "confidence": -124.1,
      "currentFrame": 87,
      "eosFrame": 84,
      "sessionId": "test",
      "tokenType": "EOS"
      "words": [
        {
          "sentenceIndex": 1,
          "textIndex": 2,
          "expected": "general",
          "recognized": "general",
          "label": "CW",
          "description": "Correct",
          "explanation": "The pronunciation matches the expected text.",
          "startTiming": 40,
          "endTiming": 320
        },
        {
          "sentenceIndex": 1,
          "textIndex": 3,
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
  ]
}
```

Name      | Type     | Description
----------|----------|------------
id        | `string` | The id of the recording.
created   | `string` | The timestamp when the recording was created.
updated   | `string` | The timestamp when the recording was last updated.
audioUrl  | `string` | The url to fetch the recorded audio.
sentences | `array`  | A list containing the feedback per sentence.


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

### URI

```
nl.itslanguage.feedback.resume
```

### Parameters

Name        | Type     | Description
------------|----------|------------
id          | `string` | **Required** The id of the feedback to resume.
sentence_id | `int`    | **Required** The id of the sentence (starting with `0`) to resume on.
