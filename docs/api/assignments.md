# Assignments

Annotation assignments provide a set of speech recordings to one (base annotator) or two (also the full annotator) specific users to annotate.

## List all assignments

A user with the 'ADMIN' role will receive all the assignments.
Non-admins will receive only the assignments assigned to them.

### URL

```http
GET /assignments HTTP/1.1
```

### Request

```http
GET /assignments HTTP/1.1
Accept: application/json
```

### Response

The response is a JSON list with assignments.
The ``digest`` field is the filename without extension for a given recording.

```http
HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "id":49847474,
    "created":"2017-08-25T14:12:15Z",
    "updated":"2018-06-10T10:24:20Z",
    "name":"Assignment #1",
    "baseDueDate":"2017-11-05T00:00:00Z",
    "fullDueDate":"2017-11-12T00:00:00Z",
    "recordings":[
       {
          "recording":"53242344042624",
          "organisation":"testschoolbarneveld",
          "challenge":"challenge1",
          "digest":"82c2b290ef674edf99af0e184dc5e6c4",
          "tenant":null
       }
    ],
    "annotations":[
       {
          "id":6236956254011392,
          "recording":{
             "recording":"53242344042624",
             "organisation":"testschoolbarneveld",
             "challenge":"challenge1",
             "digest":null,
             "tenant":"demo"
          },
          "doneBase":null,
          "doneFull":null
       }
    ]
  }
]
```


## Get a single assignment

### URL

```http
GET /assignment/:assignment HTTP/1.1
```

* `assignment` - **Required** The assignment identifier.

### Request

```http
GET /assignment/assignment_1 HTTP/1.1
Accept: application/json
```

### Response

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id":49847474,
  "created":"2017-08-25T14:12:15Z",
  "updated":"2018-06-10T10:24:20Z",
  "name":"Assignment #1",
  "baseDueDate":"2017-11-05T00:00:00Z",
  "fullDueDate":"2017-11-12T00:00:00Z",
  "recordings":[
     {
        "recording":"53242344042624",
        "organisation":"testschoolbarneveld",
        "challenge":"challenge1",
        "digest":"82c2b290ef674edf99af0e184dc5e6c4",
        "tenant":null
     }
  ],
  "annotations":[
     {
        "id":6236956254011392,
        "recording":{
           "recording":"53242344042624",
           "organisation":"testschoolbarneveld",
           "challenge":"challenge1",
           "digest":null,
           "tenant":"demo"
        },
        "doneBase":null,
        "doneFull":null
     }
  ]
}
```

## Create an assignment

### URL

```http
POST /assignments HTTP/1.1
```

### Request parameters

Parameters should be posted as JSON body.

Name             | Type     | Description
-----------------|----------|------------
`id`             | `string` | **Optional** The unique identier of the assignment. If none is given, one is generated.
`name`           | `string` | **Optional** The name for the assignment serving as guidance to the
annotator.
`baseAnnotator`  | `string` | **Optional** The unique identifier of the annotator user appointed as
base annotator.
`fullAnnotator`  | `string` | **Required** The unique identifier of the annotator user appointed as
full annotator.
`recordings`     | `list`   | **Required** The recording to include in the assignment. See below for
the format of each recording.
`baseDueDate`    | `string` | **Optional** The due date for when all recordings should be completed
by the base annnotator.
`fullDueDate`    | `string` | **Optional** The due date for when all recordings should be completed
by the full annnotator.

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


## Update an assignment

### URL

```http
PUT /assignments/:assignment HTTP/1.1
```

* `assignment` - **Required** The assignment identifier.

Update one or more properties of an existing assignment.

### Request parameters

These are exactly as specified in [Create an assignment](#create-an-assignment).

### Request

The request is exactly as specified in [Create an assignment](#create-an-assignment).

### Response

The response is exactly as specified in [Create an assignment](#create-an-assignment).
