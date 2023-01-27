# stockmq-k8s
StockMQ on Kubernetes with Helm Charts

## Prerequisites

StockMQ Server requires a running instance of NATS. 

```sh
> helm repo add nats https://nats-io.github.io/k8s/helm/charts/
> helm repo update
> helm install nats nats/nats
```

## Getting started with StockMQ using Helm

In this repo you can find the Helm 3 based [charts](https://github.com/stockmq/stockmq-k8s/tree/main/helm/charts) to install StockMQ.

```sh
> helm repo add stockmq https://stockmq.github.io/stockmq-k8s/helm/charts/
> helm repo update

> helm repo list
NAME   	URL                                                    
stockmq	https://stockmq.github.io/stockmq-k8s/helm/charts/

> helm install stockmq stockmq/stockmq-server
```
