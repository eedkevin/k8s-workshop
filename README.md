
## Prerequisite
- install k3d, a lightweight wrapper to run k3s (Rancher Lab’s minimal Kubernetes distribution) in docker.
```sh
brew insatll k3d
```

- install kubernetes-cli, aka kubectl
```sh
brew install kubernetes-cli
```

- install kubie, a much more powerful alternative to kubectx and kubens
```sh
brew install kubie
```

## Lift k8s cluster up on local
- create local docker register
```sh
k3d cluster create mycluster --registry-create mycluster-registry.localhost:5000
```

- healthcheck your local k8s cluster
```sh
kubectl get nodes
```

<!-- - create local docker registry
```sh
k3d registry create registry.localhost --port 5000
``` -->

## Prepare the docker images
- build llm-athena-api
```sh
cd <path-to-athena-api> 
docker build -t llm-athena-api . 
```
- push llm-athena-api to local docker registry
```sh
docker tag llm-athena-api localhost:5000/llm-athena-api:latest
```

```sh
docker push localhost:5000/llm-athena-api:latest
```

- build llm-athena-svc
```sh
cd <path-to-athena-svc> 
docker build -t llm-athena-svc . 
```
- push llm-athena-api to local docker registry
```sh
docker tag llm-athena-svc localhost:5000/llm-athena-svc:latest
```

```sh
docker push localhost:5000/llm-athena-svc:latest
```

## Deploy to k8s cluster
- create k8s namespace
```sh
kubectl apply -f k8s/dev/namespace.yaml
```

- switch kubectl context and namespace
```sh
kubie ctx
kubie ns dev
```

- deploy llm-athena-api
```sh
kubuctl apply -f k8s/dev/components/llm-athena-api
```

- deploy llm-athena-svc
```sh
kubuctl apply -f k8s/dev/components/llm-athena-svc
```
