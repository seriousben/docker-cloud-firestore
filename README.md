# Dockerized Google Cloud firestore Emulator

Dockerized [Google Cloud firestore Emulator](https://cloud.google.com/sdk/gcloud/reference/beta/emulators/firestore/start/).

The image is meant to be used for creating an standalone emulator for local development and testing.

## Usage

### Starting

`docker run -p "8080:8080" -e PROJECT_ID=some-project-id seriousben/docker-cloud-firestore`

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
