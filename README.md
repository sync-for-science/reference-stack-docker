# Docker reference stack

This repository provides a [Docker](https://www.docker.com/)-based technology stack, composed of many components which together emulate a research application and an electronic health record vendor. These components can be used as reference implementations for developers who wish to develop a research application that consumes electronic health record data from participants, or for EHR vendors who are exploring implementing the [SMART Health IT platform](https://smarthealthit.org/).

## Getting started

### Cloning the repository

The stack makes heavy use of [git submodules](https://git-scm.com/docs/git-submodule), so one extra step is required:

    $ git clone https://github.com/sync-for-science/reference-stack-docker
    ...
    $ cd reference-stack-docker
    $ git submodule update --init --recursive
    ...

### Running the stack

We use [Docker Compose](https://docs.docker.com/compose/) to orchestrate all the components, so starting the entire stack is simple:

    $ docker-compose up
    ...

Pulling or building the containers for the first time may take several minutes to complete, and the FHIR servers take a minute or two to start up in every case. Once everything is up and running, you should be able to access the various services and interfaces through `localhost`!

### Available services

This list shows all services and interfaces included in the reference stack, along with default ports which can be adjusted by editing `docker-compose.override.yml`.

| Name                                                                      | Description                   | Port   |
|---------------------------------------------------------------------------|-------------------------------|--------|
| [`api`](#hapi-fhir-servers-apiapi-stu3)                                   | HAPI-FHIR server (DSTU2)      | `9002` |
| [`api-stu3`](#hapi-fhir-servers-apiapi-stu3)                              | HAPI-FHIR server (STU3)       | `9005` |
| [`smart`](#smart-on-fhir-smartsmart-stu3)                                 | SMART layer (DSTU2)           | `9000` |
| [`smart-stu3`](#smart-on-fhir-smartsmart-stu3)                            | SMART layer (STU3)            | `9006` |
| [`app`](#frontend-application-app)                                        | Research application frontend | `9001` |
| [`research-app-api`](#backend-api-and-synchronizer-research-app-api)      | Research application backend  | `9005` |
| [`token-introspector`](#token-introspector-token-introspector)            | Token introspection service   | `9004` |
| [`tests`](#test-suite-tests)                                              | Sync for Science test suite   | `9003` |

## Components

### Vendor

#### HAPI-FHIR servers (`api`/`api-stu3`)

[HAPI-FHIR](http://hapifhir.io/) is an implementation of the FHIR specification written in Java. We include both STU2 and DSTU3 FHIR servers, which provide the base FHIR functionality for the stack.

#### SMART on FHIR (`smart`/`smart-stu3`)

We provide a layer that sits on top of the HAPI-FHIR servers and provides some of the functionality prescribed by [SMART on FHIR](http://docs.smarthealthit.org/). This layer acts as an EHR vendor's patient-facing interface, where a participant would be directed to in order to log into the vendor's site and consent to providing a research application with access to their data.

Configuration options in `docker-compose.override.yml`:
 * `BASE_URL`: This should be set to the URL that the service is being accessed by, which is either `http://localhost:9000`/`http://localhost:9006` when a human is using the research application interfaces, or `http://smart:5000`/`http://smart-stu3:5000` when the test suite is testing the authentication process, because the test suite accesses the SMART interfaces directly through Docker networking.
 * `ENABLE_UNSECURE_FHIR`: This determines whether the open FHIR endpoints are made available.

### Research application

#### Frontend application (`app`)

This is a simple application which provides a sample research application interface. A participant would start the data sharing process here, with a prompt to connect their electronic health records with the application which kicks off the OAuth process.

#### Backend API and synchronizer (`research-app-api`)

This application provides some of the backend functionality for the research application, and is responsibile for syncing the data between the research application and the EHR vendor. Our implementaiton syncs the data only when the access is first granted by the participant, but a real research application may want to run the synchronization process on a more regular basis.

### Utilities

#### Token introspector (`token-introspector`)

We provide a simple token introspector, which simply checks whether a token is valid for a given patient ID by trying to gain access with it. For example, you can obtain a token (after one has been granted through the normal workflow) by checking the database of the SMART layer:

    $ docker exec reference-stack-docker_smart_1 apt install sqlite3
    ...
    $ docker exec reference-stack-docker_smart_1 sqlite3 auth_proxy/db/db.sqlite "select access_token, patient_id from token"
    mYnaLGDEiZs6uORL61HAdy99uEh0Sy|smart-1288992

Then you can check that token against the SMART layer with Postman or curl:

    $ curl -X POST http://localhost:9004/api/introspect -F token=mYnaLGDEiZs6uORL61HAdy99uEh0Sy -F patient=smart-1288992
    {
      "active": true, 
      "introspected": "http://smart:5000/api/fhir", 
      "patient": {
        ...
      },
      "scope": "Patient/*.read"
    }


#### Test suite (`tests`)

The [Sync for Science test suite](https://github.com/sync-for-science/test-suite) is included in the reference stack.

#### Miscellaneous tasks (`tasks`)

The stack includes a container which is responsible for loading sample data into the initially empty FHIR server for both STU2 and DSTU3 FHIR versions. To populate this data in the FHIR server, simply use the `docker-compose run` command from the reference stack directory:

    $ docker-compose run tasks load-sample-data-stu2
    ...
    $ docker-compose run tasks load-sample-data-stu3
    ...


## Developing on the stack

Most of the stack's components are included as git submodules, with a corresponding git repository available at the [Sync for Science github site](https://github.com/sync-for-science). If you wish to develop the components, we recommend cloning the component you're interested in to your local machine, and then mounting your local repository into the Docker container. For example, if you have a local copy of the [SMART layer](https://github.com/sync-for-science/test-suite) located at `/home/s4s/auth_proxy`, you can edit `docker-compose.override.yml` to mount this as a volume and add `FLASK_DEBUG=1` to the environment (to enable auto-reload):

```yaml
services:
  ...
  smart:
    environment:
     - BASE_URL=http://localhost:9000
     - ENABLE_UNSECURE_FHIR=True
     - FLASK_DEBUG=1
    ports:
     - "9000:5000"
    volumes:
     - /home/s4s/auth_proxy:/usr/src/app
```
