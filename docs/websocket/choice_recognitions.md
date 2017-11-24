# Choice Recognitions

Performing a choice recognition is broken up into various calls.

To perform a choice recognition a number of steps have to be made:

1. [Initialising a choice recognition](#initialising-a-choice-recognition)
2. [Initialise choice challenge](#initialise-choice-challenge)
3. [Register audio procedure](#register-audio-procedure)
4. [Perform the recognition](#perform-the-recognition)


## Initialising a choice recognition

This is the starting point for a choice recognition. A unique recognition id
is generated, which serves a leading role in the recognition. Each other call
requires the recognition id as a parameter.

If other RPCs are called without this RPC being called first, the error
`nl.itslanguage.session_not_initialised` will be returned.

### URI

```
nl.itslanguage.choice.init_recognition
```

### Response

The unique recognition id is returned as a string. The id is used in other calls
to identify the current recognition.


## Initialise choice challenge

Before performing the recognition, a WFST needs to be prepared for the
challenge. When the RPC is called, the challenge is initialised asynchronously.
When the challenge is to be used, the server automatically waits for the
challenge initialisation to finish. If the initialisation results in an error,
the error is relayed to the client.

### URI

```
nl.itslanguage.choice.init_challenge
```

### Parameters

Name            | Type     | Description
----------------|----------|------------
recognition_id  | `string` | **Required** The id of the recognition to prepare the challenge for.
organisation_id | `string` | **Required** The id of the organisation in which to find the challenge.
challenge_id    | `string` | **Required** The id of the challenge to prepare.


## Register audio procedure

To send audio for recognition a WAMP procedure must be registered. Check your
WAMP library's documentation on how to do this. Some are linked here:

* [Autobahn-js][1]
* [Autobahn|Python][2]

The procedure must return [progressive results], check your library's
documentation on how to implement this. Every progressive result is a chunk of
the audio and so audio is sent in chunks.
Later on, the server will call this procedure to 'pull' the audio in. When the
procedure returns, the stream is finished/closed.

Note that a complete (self-describing) audio file is expected, starting with a
header. Check [the audio documentation page](audio.md#self-describing-formats)
to see more about supported formats.

The procedure can best be registered to a randomly generated URI, using uuid4
for this is advisable to ensure uniqueness. For every audio a separate procedure
must be registered. The URI is used later on.
It is advisable to unregister the procedure when the streaming is finished.

!!! warning
    Re-using a URI can result in conflicts.


## Perform the recognition

The recognition can be performed when all preparations are done.

### URI

```
nl.itslanguage.choice.recognise
```

### Parameters

Name           | Type     | Description
---------------|----------|------------
recognition_id | `string` | **Required** The id of the recognition to perform.
rpc            | `string` | **Required** The URI of the registered audio rpc.

### Response

A JSON object with the recognition results and metadata is returned.

Example:
```json
{"id": "g93h844",
 "audioUrl": "https://api-dot-itslanguage.nl:80/organisations/1/challenges/choice/4/recognitions/g93h844/audio",
 "recognised": "Unicorns"}
```

The RPC can return various errors:

* `nl.itslanguage.recognition_failed` If the recognition of the audio failed.
* `nl.itslanguage.no_such_organisation` If the organisation doesn't exist.
* `nl.itslanguage.no_such_challenge` If the challenge doesn't exist.


[1]: https://github.com/crossbario/autobahn-js/blob/master/doc/programming.md#registering-procedures
[2]: https://autobahn.readthedocs.io/en/latest/wamp/programming.html#registering-procedures
[progressive results]: https://crossbar.io/docs/Progressive-Call-Results/
