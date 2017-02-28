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
                "recording": {
                    "id": "rec_2",
                    "audioUrl": "http://example.com/audio.mp3"
                }
            },
            {
                "id": "chal_2",
                "audioUrl": "http://example.com/audio.mp3",
                "recording": null
            }
        ]

    }
]
```
