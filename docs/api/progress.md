# Progress

## Get progress of a category

The progress of a category (or actually, challenges in a category) can be
requested by a user. The progress is returned for the current user.

### URL

```http
GET /categories/:category/progress HTTP/1.1
```

* `category` - **Required** The category id.

### Request

```http
GET /categories/:category/progress HTTP/1.1
Accept: application/json
```

### Response

```http
HTTP/1.1 200 OK
Content-Type: application/json

[
    {
        "category": "lo_1",
        "percentage": 50,
        "user": {
            "id": "user_1",
            "firstName": "Martin",
            "infix": "van",
            "lastName": "Nostrand"
        },
        "challenges": [
            {
                "id": "chal_1",
                "audioUrl": "http://example.com/audio.mp3",
                "recordingSet": {
                  "id": "5600160f578c4db6b7b0327e16e42153",
                  "combinedAudioUrl": "http://example.com/fullaudio.mp3",
                  "recordings": [
                    {
                      "id": "rec_1",
                      "audioUrl": "http://example.com/audio.mp3"
                    },
                    {
                      "id": "rec_2",
                      "audioUrl": "http://example.com/audio.mp3"
                    }
                  ]
                }
            },
            {
                "id": "chal_2",
                "audioUrl": "http://example.com/audio.mp3",
                "recordingSet": null
            }
        ]

    }
]
```


## Get progress of a category for multiple users

Getting progress for multiple users is possible by supplying groups when
requesting progress. Progress is then fetched for users in the selected groups.

Groups can be selected by using the `group` query param. The param can be given
multiple times to select multiple groups.

```http
GET /categories/:category/progress?group=group_1&group=group_2 HTTP/1.1
Accept: application/json
```

### Filters

The following [filters](filters.md) may be applied:

Name  | Description
------|------------
role  | The id of the role that a user should be in.
