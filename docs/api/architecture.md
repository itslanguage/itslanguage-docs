# Steaming cloud architecture

{% dot architecture.svg
    digraph Architecture {
      splines=ortho // Only straight lines (horizontal/vertical)
      nodesep=1 // Prevent line overlapping
      node [
        shape=record
        style=rounded
      ]

      Tenant [label="Tenant"]
      Student [label="Student"]

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

      Student -> {API, Tenant, WSS}
      {Tenant, WSS} -> API -> {GCS, NDB}
    }
%}

## Components and actors

### Tenant

A client corporation using the ITSLanguage REST API to perform CRUD
operations on organisations, students, challenges.

### Student

An actor performing the audio recordings. A student belongs to an organisation
(a school). This organisation is a client corporation of the tenant.

A student connects with the:

* REST API to retrieve challenge metadata (e.g. reference audio).
* Websocket API to submit recorded audio.

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
