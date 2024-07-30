Presto Redis Helm Chart
==
This is a helm chart with the following features
- Presto with
    - 1 coordinator node
    - 2 worker nodes
    - A redis connector
- 2 Redis nodes
- 2 rocky linux nodes
- 1 ubuntu node with
    - Presto cli
    - Java 11 to run presto cli
- 1 python job to populate the redis node with data
## Usage
[Helm](https://helm.sh) must be installed to use the charts.

Once helm is installed, add the repo as follows:
```console
helm repo add presto-redis https://seanlam751.github.io/presto-redis/
```
To see the charts, run `helm search repo presto-redis`.
Then, you can install the chart with
```console
helm install presto-redis presto-redis/presto-redis
```
Find {presto-svc-ip} by using `kubectl get svc presto-redis` and looking at cluster-ip. Replace {presto-svc-ip} with the actual cluster ip.

To access presto using the command line interface, use the following commands
```
UBUNTU_POD_NAME=$(kubectl get pods --no-headers | grep 'ubuntu-deployment' | awk '{print $1}')
kubectl exec -it $UBUNTU_POD_NAME -- bin/bash
./presto --server {presto-svc-ip}:8080
```
To access keys and values from presto redis,
```
USE redis.default;
SELECT _key FROM "table";
SELECT _value FROM "table";
```
To access the web dashboard of presto, port forward the presto-redis service.
```
kubectl port-forward service/presto-redis 8080:8080
```
