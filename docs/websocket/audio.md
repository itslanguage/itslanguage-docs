# Audio

Various RPCs accept audio uploads. Supported audio formats are documented here.

## WAV

### Mimetypes

The WAV format has multiple mimetypes associated with it. The following are
supported by ITSLanguage:

* `audio/wav`
* `audio/wave`
* `audio/x-wav`

### Parameters

name         | type  | description
-------------|-------|------------
channels     | `int` | **Required** The number of channels.
frame_rate   | `int` | **Required** The frame rate in Hertz.
sample_width | `int` | **Required** The sample width in bits.


## Self-describing formats

A self-describing audio format has a file header with all required information.
The [SoX documentation][1] elaborates on this in more technical details.

Basically, any self-describing audio format *should* work, however only WAVE is
supported.

[1]: http://sox.sourceforge.net/AudioFormats-6.html
