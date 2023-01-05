# stockmq-k8s
StockMQ on Kubernetes with Helm Charts

## Getting started with StockMQ using Helm

In this repo you can find the Helm 3 based [charts](https://github.com/stockmq/stockmq-k8s/tree/main/helm/charts) to install StockMQ.

```sh
> helm repo add stockmq https://stockmq.github.io/stockmq-k8s/helm/charts/
> helm repo update

> helm repo list
NAME   	URL                                               
nats   	https://nats-io.github.io/k8s/helm/charts/        
stockmq	https://stockmq.github.io/stockmq-k8s/helm/charts/

> helm install stockmq stockmq/stockmq-server
```
