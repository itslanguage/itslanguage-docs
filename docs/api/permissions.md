# Permissions and roles

All users in the ITSLanguage framework have at least one role.
Along with a role comes a set of permissions.
Each API call in the system requires a certain permission.

## Roles

Roles in the ITSLanguage framework

Role        | Description
------------|------------
`STUDENT`   | A student
`ORGANISATION`   | An organisation
`TENANT`    | A tenant


## Permissions

Below are the permissions per role in a single table.

Permissions \ Roles      | TENANT | ORGANISATION | STUDENT | TEACHER | ANNOTATOR_FULL | ANNOTATOR_BASE
-------------------------|--------|--------------|---------|---------|----------------|---------------
ANNOTATION_CREATE        |        |              |         |         | X              | X
ANNOTATION_DELETE        |        |              |         |         | X              | X
ANNOTATION_EDIT          |        |              |         |         | X              | X
ANNOTATION_LIST          |        |              |         |         | X              | X
ASSIGNMENT_LIST          |        |              |         |         | X              | X
AUDIO_ANALYSIS_CREATE    | X      | X            | X       | X       |                |
AUDIO_ANALYSIS_LIST      | X      | X            | X       | X       |                |
AUDIO_RECOGNITION_CREATE |        |              | X       | X       |                |
AUDIO_RECOGNITION_LIST   |        | X            | X       | X       |                |
CATEGORY_CREATE          |        | X            |         | X       |                |
CATEGORY_LIST            |        | X            | X       | X       | X              | X
CHALLENGE_CREATE         |        | X            |         | X       |                |
CHALLENGE_DELETE         | X      | X            |         | X       |                |
CHALLENGE_EDIT           | X      | X            |         | X       |                |
CHALLENGE_LIST           | X      | X            | X       | X       | X              | X
CHOICE_CHALLENGE_CREATE  |        | X            |         |         |                |
CHOICE_CHALLENGE_DELETE  |        | X            |         |         |                |
CHOICE_CHALLENGE_LIST    |        | X            | X       | X       |                |
DOWNLOAD                 | X      | X            | X       | X       | X              | X
FEEDBACK_CREATE          |        |              | X       |         |                |
GROUP_CREATE             |        | X            |         | X       |                |
GROUP_LIST               |        | X            | X       | X       | X              | X
ORGANISATION_CREATE      | X      |              |         |         |                |
ORGANISATION_LIST        | X      |              |         |         |                |
SPEECH_RECORDING_CREATE  |        |              | X       | X       |                |
SPEECH_RECORDING_LIST    | X      | X            | X       | X       | X              | X
USER_CREATE              | X      | X            |         | X       |                |
USER_LIST                | X      | X            |         | X       | X              | X

If a call is made with an invalid scope gained from an OAuth2 Token and thus
with insufficient permissions, a `403 Forbidden` response is returned.
