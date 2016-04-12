# reference-stack-docker

To run it, you need Docker >= 1.10, and docker-compose

Edit `*.local.env` files to configure, then:

 * `docker-compose up`: launch the stack
 * `docker-compose  run tasks load-sample-data`: load sample data

Now `curl http://localhost:9000/api/fhir/Patient?` should return sample data!


---

## Deploy in prod

For example, see [./deploys/demo.syncfor.science](./deploys/demo.syncfor.science)
