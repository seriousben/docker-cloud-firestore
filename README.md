<center>

# Dockerized Google Cloud firestore Emulator

[![Docker Hub](https://img.shields.io/badge/docker-seriousben/cloud-firestore)](https://hub.docker.com/repository/docker/seriousben/cloud-firestore)

</center>

Dockerized [Google Cloud firestore Emulator](https://cloud.google.com/sdk/gcloud/reference/beta/emulators/firestore/start/).

The image is meant to be used for creating a standalone emulator for local development and testing.

## Usage

### Starting

```sh
$ docker run -p "8080:8080" -e PROJECT_ID=some-project-id seriousben/docker-cloud-firestore

Updated property [core/project].
Executing: /google-cloud-sdk/platform/cloud-firestore-emulator/cloud_firestore_emulator start --host=0.0.0.0 --port=8080
[firestore] API endpoint: http://0.0.0.0:8080
[firestore] If you are using a library that supports the FIRESTORE_EMULATOR_HOST environment variable, run:
[firestore]
[firestore]    export FIRESTORE_EMULATOR_HOST=0.0.0.0:8080
[firestore]
[firestore] Dev App Server is now running.
[firestore]
```

### Connecting to the emulator

Most client libraries support connection to the emulator.

Typically, the `FIRESTORE_EMULATOR_HOST` environment variable needs to be set to the emulator address for the cloud firestore client to connect 
to the emulator automatically.

The `PROJECT_ID` passed to the emulator and the one used to connect using the client library also have to match.

Example:

```sh
export PROJECT_ID=some-project-id
export FIRESTORE_EMULATOR_HOST=localhost:8080
./my-app-binary
```

## With Docker Compose

```yaml
version: "3.0"

services:
  firestore:
    image: seriousben/docker-cloud-firestore
    environment:
      - PROJECT_ID=project-test
  app:
    image: your-app-image
    environment:
      - FIRESTORE_EMULATOR_HOST=firestore
      - MY_APP_PROJECT_ID=project-test
```

## Implementation details

### Environment

The following environment variables must be set:

- `PROJECT_ID`: The ID of the Google Cloud project for the emulator.

### Custom commands

This image contains a script named `start-firestore` (included in the PATH). This script is used to initialize the firestore emulator.

#### Starting an emulator

By default, the following command is called:

```sh
start-firestore
```

## See Also

Lots of parts and inspiration were taken from:
* https://github.com/GoogleCloudPlatform/cloud-sdk-docker
* https://github.com/perrystallings/firestore-emulator-docker
