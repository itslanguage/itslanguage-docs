# WAMP

The websocket API is implemented using [WAMP] (Web Application Messaging
Protocol). This allows for RPCs over websockets.
To be able to use the websocket API, a WAMP client should be used.
The recommended WAMP client is Autobahn since it's from the author of WAMP.
Other [implementations](http://wamp-proto.org/implementations/) are also
available.

[WAMP]: http://wamp-proto.org


## Register audio procedure

To send audio, a WAMP procedure must be registered. Check your
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
