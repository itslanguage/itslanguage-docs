# Annotations

Annotations are notes to specific fragments of audio. These annotations can then be classified.
Common classifiers are `Prompt`, `Out of Prompt` or `Error`. Annotations are made by users called
annotators. They are able to put these annotations on recorded audio.

An annotator either has the role of `full` annotator of `base` annotator. Base annotators only
have a subset of labels available to annotate with. The more senior `full` annotator can correct
the base annotators annotation and has an extended set of labels available.

## List all annotations

A user with the 'ADMIN' role will receive all the annotations.
Non-admins will receive only the annotations assigned to them.
See below for further filtering options.

### URL

```http
GET /annotations HTTP/1.1
```

### Request parameters

The are no request parameters

```http
GET /annotations?recording=1 HTTP/1.1
Accept: application/json
```

### Filters

The following [filters](filters.md) may be applied:

Name          | Type     | Description
--------------|----------|------------
`annotator`   | `string` | The unique identier of the annotator to find the annotations for.
`assignment`  | `string` | The assignment for which to find the annotations.
`recording`   | `string` | The speech recording for which to find the annotations.
`done`        | `bool`   | Whether or not and how to filter on the done property. None when all values are allowed, True if only done annotations are desired. False for all not finished annotations.
`perPage`     | `string` | Amount of annotations to return.
`startCursor` | `string` | A previously returned cursor to use as an 'offset'.

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
       "challenge":"challenge1",
       "digest":null,
        "tenant":"demo"
    },
    "audioUrl":"https://api.itslanguage.io/download/2257a2f8ada1446bae8d97f591169de9",
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
  "id":5646929551687680,
  "updated":"2018-06-07T13:50:44Z",
  "created":"2018-06-07T13:50:44Z",
  "assignment":4873731453222912,
  "doneBase":null,
  "doneFull":null,
  "recording":{
     "recording":"5715999101812736",
     "organisation":"aschool",
     "challenge":"challenge1",
     "digest":null,
     "tenant":"demo"
  },
  "audioUrl":"https://api.itslanguage.io/download/2257a2f8ada1446bae8d97f591169de9",
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
```

## Create an annotation

### URL

```http
POST /annotations HTTP/1.1
Accept: application/json
Content: application/json

{
   "annotation":"5646929551687680",
   "assignment":4873731453222912,
   "recording":{
      "recording":"5715999101812736",
      "organisation":"org1",
      "challenge":"challenge1",
      "tenant":"demo"
   },
   "fragments":[
      {
         "start":0.59,
         "end":1.07,
         "labels":[
            {
               "layer":"Words",
               "label":"zon",
               "comment":""
            },
            {
               "layer":"OutOfPrompt",
               "label":"InsertionUtterance",
               "comment":"zo"
            }
         ]
      },
      {
         "start":9.81,
         "end":10.93,
         "labels":[
            {
               "layer":"Words",
               "label":"weer.",
               "comment":""
            }
         ]
      },
   ],
}
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

The Recording format (`FlatRecordingReference`). All parameters together form the full namespace of
one audio recording.

Parameters should be posted as JSON body.

Name             | Type     | Description
-----------------|----------|------------
`tenant`         | `string` | **Required** The unique identier of the tenant.
`organisation`   | `string` | **Required** The identifier of the organisation within the tenant
namespace.
`challenge`      | `string` | **Required** The identifier of the challenge as specified within the
organisation..
`recording`      | `string` | **Required** The identifier of the recording as specified within the
organisation.

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
`layer`   | `string` | **Required** The specifier for the layer (e.g. `Words`, `OutOfPrompt`,
`Error`, `Noise`).
`label`   | `string` | **Required** The specifier of a particular label within a layer (on the Words layer we would expect the word here).
`comment` | `string` | **Required** An optional extra value.

### Response

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
   "updated":"2019-04-14T07:55:16Z",
   "created":"2018-06-07T13:50:44Z",
   "id":5646929551687680,
   "doneBase":null,
   "doneFull":null
   "assignment":4873731453222912,
   "recording":{
      "recording":"5715999101812736",
      "organisation":"org1",
      "challenge":"challenge1",
      "digest":null,
      "tenant":"demo"
   },
   "audioUrl":"https://api.itslanguage.io/download/3357a2f8ada1446bae8d97f591169de9",
   "fragments":[
      {
         "start":0.59,
         "end":1.07,
         "labels":[
            {
               "layer":"Words",
               "label":"zon",
               "comment":""
            },
            {
               "layer":"OutOfPrompt",
               "label":"InsertionUtterance",
               "comment":"zo"
            }
         ]
      },
      {
         "start":9.81,
         "end":10.93,
         "labels":[
            {
               "layer":"Words",
               "label":"weer.",
               "comment":""
            }
         ]
     }
   ]
}
```

## Update an annotation

### URL

```http
PUT /annotations/:annotation HTTP/1.1
```

* `annotation` - **Required** The annotation identifier.

Update one or more properties of an existing annotation.

### Request parameters

These are exactly as specified in [Create an annotation](#create-an-annotation).

### Request

The request is exactly as specified in [Create an annotation](#create-an-annotation).

### Response

The response is exactly as specified in [Create an annotation](#create-an-annotation).


# Reference Annotations

A reference annotation is an annotation for a specific audio recording, usually generated through
forced alignment (matches words in the prompt of the challenge to the audio recording).
That leads to annotations on the fragments where a relation between prompt and audio was found
(the alignment). No error or out of prompt classes are assigned in an automated fashion. This is
the starting point to create human annotations.

## Get a single reference annotation

When a reference annotation has been added for a specific recording at an earlier time, this is how
to retrieve it. The URL includes the full namespace to the recording.

### URL

```http
GET /organisations/:organisation/challenges/speech/:challenge/recordings/:recording/reference HTTP/1.1
```
### Request parameters

These are exactly as specified in [Get a single annotation](#get-a-single-annotation).

### Request

The request is exactly as specified in [Get a single annotation](#get-a-single-annotation).

### Response

The response is exactly as specified in  [Get a single annotation](#get-a-single-annotation).


## Update a reference annotation

There's no POST API call to create an annotation. It's basically a property of a recording to update,
therefore it's a PUT call. Due to different permissions it requires (an admin would need to
impersonate), it's not part of the recording PUT.

### URL

```http
PUT /organisations/:organisation/challenges/speech/:challenge/recordings/:recording/reference HTTP/1.1
```

### Request parameters

Parameters should be posted as JSON body.

Name          | Type   | Description
--------------|--------|------------
`annotations` | `list` | **Required** The annotations as list of type `Fragment` (as specified
above).

### Response

The response is exactly as specified in [Create an annotation](#create-an-annotation).
