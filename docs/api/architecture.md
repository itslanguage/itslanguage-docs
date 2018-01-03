# Steaming cloud architecture

{% dot architecture.svg
    digraph Architecture {
      splines=ortho // Only straight lines (horizontal/vertical)
      nodesep=1 // Prevent line overlapping
      node [
        shape=record
        style=rounded
      ]

      subgraph clusterGoogleCloud {
        label="Google cloud"
        subgraph clusterGoogleAppEngine {
          label="Google App Engine"
          API [label="Rest API"]
        }
        subgraph clusterGoogleComputeEngine {
          label="Google Compute Engine"
          WSS [label="Websocket server"]
        }
        GCS [label="Google Cloud Storage"]
        NDB [label="Google Cloud Datastore"]
      }

      edge [arrowhead=none]

      Client -> {API, WSS}
      WSS -> API -> {GCS, NDB}
    }
%}

## Components and actors

### Client

A client is any client application that interacts with the ITSLanguage
environment.
A client can use the [Rest API](#api) to perform CRUD operations.
A client can also use the [Websocket API](#websocket-server) to submit recorded
audio.

A client is typically used by a student, organisation (admin) or tenant (admin).

### Websocket server

The websocket server is listening for connections to receive and process audio.
The websocket server is located in the Google cloud and is hosted using Google
Compute Engine.

### API

The Rest API provides endpoints to perform administrative tasks.
Students, organisations, tenants and admins all use the API.

### Google Cloud Storage

Storage for submitted audio.

### Google Cloud Datastore

NoSQL datastore for all non-binary data.
