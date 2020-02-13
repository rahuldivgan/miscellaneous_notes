# Deploy fake mini-microservices

This deploys 3 microservices, all listening on port 80, and responding a 200 OK with a message of what service it is
1. frontend
2. backend
3. database

```
for DEPLOYMENT_TYPE in \
  frontend \
  microservice \
  database\
  ; do
  DEPLOYMENT="test-${DEPLOYMENT_TYPE}"

  kubectl run "${DEPLOYMENT}" \
    --image=busybox \
    --labels=app=web,role="${DEPLOYMENT_TYPE}" \
    --requests='cpu=10m,memory=32Mi' \
    --expose \
    --port 80 \
    -- sh -c "while true; do { printf 'HTTP/1.1 200 OK\r\n\n I am a ${DEPLOYMENT_TYPE}\n'; } | nc -l -p  80; done"

  kubectl scale deployment "${DEPLOYMENT}" --replicas=3
done
```
