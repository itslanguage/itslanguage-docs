# Steaming cloud architecture

{% dot architecture.svg
    digraph Architecture {
      splines=ortho // Only straight lines (horizontal/vertical)
      nodesep=1 // Prevent line overlapping
      node [
        shape=record
        style=rounded
      ]

      GCS [label="Google Cloud Storage"]
      GAE [label="Google App Engine"]
      Tenant [label="Tenant"]
      Student [label="Student"]
      GCE [label="Google Managed VMs"]
      NDB [label="Google Cloud Datastore"]

      edge [arrowhead=none]

      Student -> {GAE, GCE, Tenant}
      {GCE, Tenant} -> GAE -> {GCS, NDB}
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

### Google Managed VMs

* Websocket server listening for students connecting to receive and process audio.

### Google App Engine

REST API to interface with tenants and students performing administrative tasks.

### Google Cloud Storage

Storage for submitted audio.

### Google Cloud Datastore

NoSQL datastore for all non-binary data.
