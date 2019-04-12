# Request and Response Format

Most endpoints accept and return [JSON](#json). Some endpoints accept
[multipart form data](#multipart-form-data) to support submitting binary data
(files for example). All endpoints should return [JSON](#json) for ease of
processing.

## JSON

As described in [RFC 4627](http://www.ietf.org/rfc/rfc4627.txt):
> JSON text SHALL be encoded in Unicode. The default encoding is UTF-8.

Therefore, specifying the encoding with `charset=utf-8` in the header is
optional, it's an implicit default.

When sending JSON the `Content-Type` header needs to be set to
`application/json`. Example using curl:

```bash
curl -d '{"name": "sheffield"}' -H "Content-Type: application/json" \
  https://api.itslanguage.io/organisations
```

Blank fields are included as `null` instead of being omitted.

## Multipart form data

When submitting binary data, like audio fragments, this request format needs to
be used. An example form POST looks like this:

```http
POST /organisation/crimson/challenges/choice HTTP/1.1
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

Repeated fieldnames are accepted and transformed as lists in the given order.

More information about this format can be found in
[RFC 2388](https://www.ietf.org/rfc/rfc2388.txt).
