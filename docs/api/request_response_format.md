# Request and Response Format

All endpoints accept and return JSON, with the exception of a few special cases that require submitting blobs.

## JSON

All endpoints accept and return JSON, with the exception of a few special cases designed for use from a web browser or containing blobs.

As described in [RFC 4627](http://www.ietf.org/rfc/rfc4627.txt):
*JSON text SHALL be encoded in Unicode. The default encoding is UTF-8.*
Therefore, specifying the encoding with `charset=utf-8` in the header is optional, it's an implicit default.

When sending JSON the appropriate Content-Type header needs to be set.

```bash
curl -d '{"name": "sheffield"}' -H "Content-Type: application/json" https://api.itslanguage.nl/organisations
```

Blank fields are included as `null` instead of being omitted.

## Multipart form data

When submitted forms contains binary data like audio fragments, this request format needs to be used.
An example form POST looks like this:

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

## URL encoded forms

This content type is unsupported for most API calls, it may be supported for legacy reasons.

```http
POST /organisations/crimson/challenges/choice HTTP/1.1
Accept: application/json
Content-Type: application/x-www-form-urlencoded

question=The%20Pitcairn%20Islands%20are%20...%20in%20the%20South%20Pacific%20Ocean&choices=located&choices=sited&choices=stationed&choices=settled
```
