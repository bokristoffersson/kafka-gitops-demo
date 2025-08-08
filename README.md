# kafka-gitops-demo


Create k3d cluster
```
k3d cluster create kafka-demo --port "8080:80@loadbalancer" --port "8443:443@loadbalancer"
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
