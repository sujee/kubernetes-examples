# Deployment and Rollout

## Build Custom Nginx Images

```bash
    $   (cd  app-v1;  docker build . -t sujee/nginx:1)
    $   (cd  app-v2;  docker build . -t sujee/nginx:2)
    $   (cd  app-v3;  docker build . -t sujee/nginx:3)

    # testing
    $   docker run -d --rm  -p8001:80  sujee/nginx:1
    $   docker run -d --rm  -p8002:80  sujee/nginx:2
    $   docker run -d --rm  -p8003:80  sujee/nginx:3

    $   curl localhost:8001
    $   curl localhost:8002
    $   curl localhost:8003

    # stop the docker containers
```

## Deployment

file : [1-deployment.yaml](1-deployment.yaml)

```bash
    $   kubectl  apply -f   1-deployment.yaml

    $   kubectl get deployments

    $   kubectl get nodes -o wide

```

## Create a Service

file : [2-service.yaml](2-service.yaml)

We create a service and use nodeport to expose the service

```bash
    $   kubectl -f 2-service.yaml

    $   kubectl get svc

    $   kubectl  describe svc nginx-service
```

Access the service nodeport ip

```bash
    $   kubectl get pods -o wide

    # pick an ip one of the pods is listening on
    # say 1.2.3.4

    # don't forget the / at the end!
    $   curl 1.2.3.4:30163/
    # should see 'v1' message
```

## Update Image

```bash
    $   kubectl replace -f 3-deployment-update.yaml 

    $   kubectl rollout status deployment nginx-deployment

    $   kubectl  describe deployment nginx-deployment
```

Access and see the newer version

```bash
    # don't forget the / at the end!
    $   curl 1.2.3.4:30163/
    # should see 'v2' message
```

## Cleanup

```bash
    # delete service
    $   kubectl delete svc nginx-service

    # delete deployment
    $   kubectl delete deployment nginx-deployment
```