# Use and deploy this stack

To run it, you need Docker >= 1.10, and docker-compose

## Clone this repo with submodules
 * `git clone --recursive https://github.com/sync-for-science/reference-stack-docker`

## Update submodules
 * `git submodule update --init --recursive`

# Configure and run locally

Edit `docker-compose.override.yml` as needed to configure your preferred local port mappings, then:

 * `docker-compose up`: launch the stack
 * `docker-compose  run tasks load-sample-data`: load sample data

Now `curl http://localhost:9000/api/fhir/Patient?` should return sample data!

By default, you'll have servers running at ports:

 * `9000`: SMART EHR server
 * `9001`: Demo app server
 * `9002`: For debugging only, the internal HAPI FHIR server
 * `9003`: For testing only, the test suite client

---

# Deploy in prod

For example, see [./deploys/demo.syncfor.science](./deploys/demo.syncfor.science)
