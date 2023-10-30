# Overview

The npm scripts utilize a demo project called "emulator-sandbox".

This projects is configured to run emulators for:

- Authentication (Port 9099)
- Firestore (Port 8081)
- Cloud Storage (Port 9199)
- Pub/Sub (Port 8085)
- Eventarc (Port 9299)

NOTE: The Cloud Functions emulator must be run in the project with your Functions code; therefore, it is not included here.

# Script Descriptions

- `start:all` - Runs all emulators without pre-loaded data

- `start:import` - Runs all emulators and pre-loads data from `exports` folder but does not save changes to data or files

- `start:import:export` - Runs all emulators and pre-loads data from `exports` folder plus performs a new export over the existing export when the emulators are shutdown

- `start:firestore` - Runs only the Firestore emulator. Any emulator can be run individually in this manner

- `export` - Performs a manual export from a running Firestore and Cloud Storage emulator

# Authentication Emulator

To connect an application to the Authentication emulator requires:

1. Environment variable assignment for FIRESTORE_EMULATOR_HOST=ip:port
2. Admin initialized with appropriate project

```shell
export FIREBASE_AUTH_EMULATOR_HOST="127.0.0.1:9099"
```

```shell
# Fish shell
set -xg FIREBASE_AUTH_EMULATOR_HOST "127.0.0.1:9099"
```

```js
admin.initializeApp({ projectId: 'emulator-sandbox' })
```

# Firestore Emulator

To connect an application to the Firestore emulator requires:

1. Environment variable assignment for FIRESTORE_EMULATOR_HOST=ip:port
2. Admin initialized with appropriate project

```shell
export FIRESTORE_EMULATOR_HOST="127.0.0.1:8081"
```

```shell
# Fish shell
set -xg FIRESTORE_EMULATOR_HOST "127.0.0.1:8081"
```

```js
admin.initializeApp({ projectId: 'emulator-sandbox' })
```

Firestore running in an emulator can have all data cleared by using an HTTP DELETE call.

```shell
$ curl -v -X DELETE "http://localhost:8081/emulator/v1/projects/emulator-sandbox/databases/(default)/documents"
```

Firestore rules can be edited in `firestore.rules`.

Firestore indexes can be edited in `firestore.indexes.json`.

# Cloud Storage Emulator

To connect an application to the Cloud Storage emulator requires:

1. Environment variable assignment for FIREBASE_STORAGE_EMULATOR_HOST=ip:port
2. Admin initialized with appropriate project

```shell
export FIREBASE_STORAGE_EMULATOR_HOST="127.0.0.1:9199"
```

```shell
# Fish shell
set -xg FIREBASE_STORAGE_EMULATOR_HOST "127.0.0.1:9199"
```

```js
admin.initializeApp({ projectId: 'emulator-sandbox' })
```

Cloud Storage rules can be edited in `storage.rules`.

# Pub/Sub Emulator

To connect an application to the Pub/Sub emulator requires:

1. Environment variable assignment for PUBSUB_EMULATOR_HOST=ip:port
2. Run start:json-server script

```shell
export PUBSUB_EMULATOR_HOST="127.0.0.1:8085"
```

```shell
# Fish shell
set -xg PUBSUB_EMULATOR_HOST "127.0.0.1:8085"
```

```shell
# Create a topic
curl -s -X PUT 'http://localhost:8085/v1/projects/emulator-sandbox/topics/GenericTopic'

# Create a subscription
curl -s -X PUT 'http://localhost:8085/v1/projects/emulator-sandbox/subscriptions/GenericTopicSubscription' \
    -H 'Content-Type: application/json' \
    --data '{"topic":"projects/emulator-sandbox/topics/GenericTopic","pushConfig":{"pushEndpoint":"http://localhost:8080/projects/emulator-sandbox/topics/GenericTopic"}}'

# Post a message
curl -s -X POST 'http://localhost:8085/v1/projects/emulator-sandbox/topics/GenericTopic:publish' \
    -H 'Content-Type: application/json' \
    --data '{"messages":[{"data":"eyJmb28iOiJiYXIifQ=="}]}'
```

# References

[Firebase Emulators](https://firebase.google.com/docs/emulator-suite)

[Google Python PubSub](https://cloud.google.com/pubsub/docs/emulator#using_the_emulator)

```

```
