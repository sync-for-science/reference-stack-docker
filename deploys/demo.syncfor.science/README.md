## Deploy to demo.syncfor.science


### Prepare the remote docker-machine

```
eval $(docker-machine env demo-host)
docker network create nginx-proxy
docker-machine scp -r  deploys/demo.syncfor.science/config/ demo-host:/
```

### Launch the SMART stack

```
docker-compose \
  -f docker-compose.yml \
  -f deploys/demo.syncfor.science/docker-compose.yml \
  up -d

docker-compose \
  run tasks load-sample-data
docker-compose \
  run tasks register-client test-suite demo-secret-s4s "https://tests.demo.syncfor.science/authorized/"
docker-compose \
  run tasks register-client research-app demo-secret-s4s "https://app.demo.syncfor.science/authorized"
```



### Launch the letsencrypt proxy

```
docker-compose \
  -f deploys/demo.syncfor.science/nginx-ssl-proxy/docker-compose.yml \
  up -d
```

### See it in action

https://demo.syncfor.science/fhir-app/launch.html?iss=https://portal.demo.syncfor.science/api/fhir
