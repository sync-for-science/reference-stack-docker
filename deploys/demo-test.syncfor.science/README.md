## Deploy to demo-test.syncfor.science


### Prepare the remote docker-machine

```
eval $(docker-machine env s4s-demo-test)
docker network create nginx-proxy
docker-machine scp -r  deploys/demo-test.syncfor.science/config/ s4s-demo-test:/
```

### Launch the SMART stack

```
docker-compose \
  -f docker-compose.yml \
  -f deploys/demo-test.syncfor.science/docker-compose.yml \
  up -d

docker-compose \
  run tasks load-sample-data
docker-compose \
  run tasks register-client test-suite demo-secret-s4s "https://tests.demo-test.syncfor.science/authorized/"
docker-compose \
  run tasks register-client research-app demo-secret-s4s "https://app.demo-test.syncfor.science/authorized"
```



### Launch the letsencrypt proxy

```
docker-compose \
  -f deploys/demo-test.syncfor.science/nginx-ssl-proxy/docker-compose.yml \
  up -d
```

### See it in action

https://demo-test.syncfor.science/fhir-app/launch.html?iss=https://portal.demo-test.syncfor.science/api/fhir
