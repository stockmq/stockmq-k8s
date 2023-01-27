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

## Verify installation

Run these commands:

```sh
> kubectl get pods
NAME                                      READY   STATUS    RESTARTS   AGE
nats-0                                    3/3     Running   0          4m53s
nats-box-75695747f9-56mtf                 1/1     Running   0          4m53s
stockmq-stockmq-server-7956df8494-jrlxv   1/1     Running   0          4m15s

> kubectl logs stockmq-stockmq-server-7956df8494-jrlxv
2023/01/27 11:05:44 [INF] Starting StockMQ Server
2023/01/27 11:05:44 [INF] Starting Monitor on 0.0.0.0:9100 tls: false
2023/01/27 11:05:44 [INF] Starting GRPC on 0.0.0.0:9101 tls: false
2023/01/27 11:05:44 [INF] Starting NATS connection to nats://nats:4222
```

## Add Binance stream

Create a new file values.yaml

```yaml
config:
  websocket:
    - name: Binance-BTCUSD
      url: wss://stream.binance.com:9443/ws
      handler: Binance
      dialTimeout: 4
      retryDelay: 3
      pingTimeout: 60
      readLimit: 655350
      initMessage: >-
        {"id": 0, "method": "SUBSCRIBE", "params": ["btcusdt@kline_1s", "btcusdt@depth"]}
```

## Subscribe to Binance stream

You can use nats-box to test produced streams.

```sh
> kubectl exec -n default -it deployment/nats-box -- /bin/sh -l
> nats sub "C.*.BTCUSDT.Binance-BTCUSD"
11:19:55 Subscribing on C.*.BTCUSDT.Binance-BTCUSD 
[#1] Received on "C.1s.BTCUSDT.Binance-BTCUSD"
{"symbol":"BTCUSDT","source":"Binance-BTCUSD","time":1674818395000000,"time_srv":1674818396001000,"time_rcv":1674818396190967,"interval":"1s","open":"22931.53000000","high":"22932.13000000","low":"22931.38000000","close":"22931.42000000","volume":"2.26796000"}
```
