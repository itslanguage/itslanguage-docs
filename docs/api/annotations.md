# Annotations

An annotation assigns pronunciation errors to specific fragments of an audio recording performed the
an annotator user.

## List all annotations

A user with the 'ADMIN' role will receive all the annotations.
Non-admins will receive only the annotations assigned to them.

### URL

```http
GET /annotations HTTP/1.1
```

### Request parameters

Name          | Type     | Description
--------------|----------|------------
`annotator`   | `string` | **Optional** The unique identier of the annotator to find the annotations for.
`assignment`  | `string` | **Optional** The assignment for which to find the annotations.
`recording`   | `string` | **Optional** The speech recording for which to find the annotations.
`done`        | `bool`   | **Optional** Whether or not and how to filter on the done property. None when all values are allowed, True if only done annotations are desired. False for all not finished annotations.
`perPage`     | `string` | **Optional** Amount of annotations to return.
`startCursor` | `string` | **Optional** A previously returned cursor to use as an 'offset'.

```http
GET /annotations HTTP/1.1
Accept: application/json
```

### Response

The response is a JSON list with annotations.

```http
HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "id":5646929551687680,
    "updated":"2018-06-07T13:50:44Z",
    "created":"2018-06-07T13:50:44Z",
    "assignment":4873731453222912,
    "doneBase":null,
    "doneFull":null,
    "recording":{
       "recording":"5715999101812736",
       "organisation":"aschool",
       "challenge":"Assign_1_1_lijn3_thema9_06_EenzonvoorJet_a",
       "digest":null,
        "tenant":"demo"
    },
    "audioUrl":"https://api.itslanguage.nl/download/2257a2f8ada1446bae8d97f591169de9",
    "fragments":[
       {
         "start":5.81,
         "labels":[
            {
               "comment":"",
               "layer":"Words",
               "label":"speelt"
            }
         ],
         "end":6.45
      },
      {
         "start":6.67,
         "labels":[
            {
               "comment":"",
               "layer":"Words",
               "label":"op"
            }
         ],
         "end":7.17
      }
    ]
  }
]
```


## Get a single annotation

### URL

```http
GET /annotation/:annotation HTTP/1.1
```

* `annotation` - **Required** The annotation identifier.

### Request

```http
GET /annotation/annotation_1 HTTP/1.1
Accept: application/json
```

### Response

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  ...
}
```


## Create an annotation

### URL

```http
POST /annotations HTTP/1.1
```

### Request parameters

Parameters should be posted as JSON body.

Name             | Type     | Description
-----------------|----------|------------
`id`             | `string` | **Optional** The unique identier of the annotation. If none is given, one is generated.
`doneBase`       | `string` | **Optional** The datetime this annotation was marked as done by the base annotator.
`doneFull`       | `string` | **Optional** The datetime this annotation was marked as done by the full annotator.
`recording`      | `string` | **Required** The reference to the recording (type
`FlatRecordingReference` as specified below.
`assignment`     | `string` | **Required** The unique identifier of the assignment this annotation
is for.
`fragments`      | `list`   | **Required** The annotated fragments (type `Fragment` as specified below).

The Recording format (`FlatRecordingReference`).

Parameters should be posted as JSON body.

Name             | Type     | Description
-----------------|----------|------------
`tenant`         | `string` | **Required** The unique identier of the tenant.
`organisation`   | `string` | **Required** The unique identifier of the organisation.
`recording`      | `string` | **Required** The unique identifier of the recording.
`challenge`      | `string` | **Required** The unique identifier of the challenge.


The Fragment format:

Parameters should be posted as JSON body.

Name     | Type    | Description
---------|---------|------------
`start`  | `float` | **Required** The start point of the fragment, in seconds.
`end`    | `float` | **Required** The end point of the fragment, in seconds.
`labels` | `list`  | **Required** The labels assigned to this fragment (type `Label` as specified
below).


The Label format:

Parameters should be posted as JSON body.

Name      | Type     | Description
----------|----------|------------
`layer`   | `string` | **Required** The specifier for the layer (Words, OutOfPrompt, Error, Noise).
`label`   | `string` | **Required** The specifier of a particular label within a layer (Toevoeging ZN, Toevoeging Lidwoord, etc).
`comment` | `string` | **Required** An optional extra value (e.g. when the label is Toevoeging Lidwoord: De, Het, Een).


## Update an annotation

### URL

```http
PUT /annotations/:annotation HTTP/1.1
```

* `annotation` - **Required** The annotation identifier.

Update one or more properties of an existing annotation.

### Request parameters

XXX


### Request

XXX

### Response

XXX
