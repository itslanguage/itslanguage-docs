# Recordings

This document describes some administrative features of the recording api that
has been put behind `/recordings`.

More regular user actionable information can be found on the
[Speech Recordings](speech_recordings.md) page.

Basic idea is that an admin user is able to gather all recordings at once,
without any hassle.

## List all speech recordings

Query and return all recordings based on the scope of the authenticated user.

The following scopes are supported:

- tenant
- organisation
- user

Using the scopes this means that it is possible to get all recordings for a 
tenant, an organisation or user.

### URL

```http
GET /recordings HTTP/1.1
```

### Request

```http
GET /recordings HTTP/1.1
Accept: application/json
```

### Response

The response is a JSON list with speech recordings.

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  recordingsCount: 1,
  recordings: [
    {
      "challenge": {
          "id": "some_challenge_id",
          "organisationId": "organisation",
          "language": "nld",
          "srtUrl": "https://api.itslanguage.io/challenges/speech/some_challenge_id/srt?access_token=..."
      },
      "updated": "2020-07-01T12:00:00Z",
      "created": "2020-07-01T12:00:00Z",
      "resultSet": {
          "id": "11d6c30a791446drat929dg50g90g9g4",
          "combinedFeedback": [],
          "combinedAudioUrl": "http://its-gae.itsl:8080/resultset/11d6c30a791446drat929dg50g90g9g4/audio"
      },
      "id": "497fa21edd294j05b3ae4062366bce2e",
      "userId": "user1",
      "audioUrl": "https://api.itslanguage.nl/download/ef6b506d2f474gcb9ha20u8e46cb3ar3"
    },
  ]
}
```

### Filters

The following [parameters](parameters.md) may be applied:

Name | Type | Default value | Description
-----|------|---------------|------------
recordings | `bool` | `true` | **Optional** Embed the recordings in the result.
