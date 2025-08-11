# kafka-gitops-demo


Create k3d cluster
```
k3d cluster create kafka-demo --port "8080:80@loadbalancer" --port "8443:443@loadbalancer"
```
Install flux crds... (Will be done when bootstraping, but if you would like to test separate files you need to install the flux CRDs)
```
flux install
```

For FluxCD you need to fork this repo, see [Flux bootstrap for GitHub](https://fluxcd.io/flux/installation/bootstrap/github/)
```
export GITHUB_TOKEN=<token>
export GITHUB_USER=<github-user>
export GITHUB_REPO=kafka-gitops-demo
```

Bootstrap Flux
```
flux bootstrap github \
    --context=k3d-kafka-demo \
    --owner=${GITHUB_USER} \
    --repository=${GITHUB_REPO} \
    --branch=main \
    --personal \
    --path=clusters/local
```

## Test
Connect a producer
```
kubectl  run kafka-producer -ti --image=quay.io/strimzi/kafka:0.47.0-kafka-4.0.0 --rm=true --restart=Never -- bin/kafka-console-producer.sh --bootstrap-server my-cluster-kafka-bootstrap:9092 --topic demo-topic
```

Connect a consumer
```
kubectl run kafka-consumer -ti --image=quay.io/strimzi/kafka:0.47.0-kafka-4.0.0 --rm=true --restart=Never -- bin/kafka-console-consumer.sh --bootstrap-server my-cluster-kafka-bootstrap:9092 --topic demo-topic --from-beginning
```
