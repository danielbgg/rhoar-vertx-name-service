# RHOAR Vert.x Name Service
Vert.x Name Service Microservice - RHOAR course

## Compile Application
```
mvn clean package
```

## Openshift Deployment
```
export CIRCUITBREAKER_PROJECT_NAME=circuitbreaker-demo
oc new-project $CIRCUITBREAKER_PROJECT_NAME
mvn clean fabric8:deploy -Popenshift
```

### Deployment Test
```
export GREETING_SERVICE_URL=http://$(oc get route greeting-service -n $CIRCUITBREAKER_PROJECT_NAME -o template --template='{{.spec.host}}')
export NAME_SERVICE_URL=http://$(oc get route name-service -n $CIRCUITBREAKER_PROJECT_NAME -o template --template='{{.spec.host}}')
curl -X GET "${GREETING_SERVICE_URL}/api/greeting"
curl -X PUT -H "Content-Type: application/json" -d '{"state": "fail"}' "${NAME_SERVICE_URL}/api/state"
curl -X GET "${GREETING_SERVICE_URL}/api/greeting"
curl -X PUT -H "Content-Type: application/json" -d '{"state": "ok"}' "${NAME_SERVICE_URL}/api/state"
curl -X GET "${GREETING_SERVICE_URL}/api/greeting"
```
