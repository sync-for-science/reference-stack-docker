# reference-stack-docker

To run it, you need Docker >= 1.10, and docker-compose

Edit `*.local.env` files to configure, then:

 * `docker-compose up`: launch the stack
 * `docker-compose  run tasks load-sample-data`: load sample data

Now `curl http://localhost:9000/api/fhir/Patient?` should return sample data!


---

## Deploy in prod

1. Provision a host using `docker-machine create myhost`
2. Create local config at `deploys/myhost` (see example)
3. Copy config to the docker-machine host using `docker-machine scp deploys/myhost myhost:/`
4. Launch the stack via

```
docker-compose -f docker-compose.yml \
               -f deploys/myhost/docker-compose.yml
               up
```

5. Add sample data vai `docker-compose run tasks load-sample-data`
