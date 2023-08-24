# Kube Programming
In this repo we will be programming different items in kubernetes cluster

## Exercise 1
### Learning about api
#### API 
Api is the central communication medium of the kubernetes cluster. Everything about kubernetes resources can be queried through the api calls.

#### Cluster
Create a kind cluster using this [config](./Kind/Config.yaml). In this we are exposing the local api endpoint at port ` 6443`. 

#### Hitting the endpoint
After the cluster is up we can query the api at `https://127.0.0.1:6443` 
```bash
curl https://127.0.0.1:6443
curl failed to verify the legitimacy of the server and therefore could not
establish a secure connection to it. To learn more about this situation and
how to fix it, please visit the web page mentioned above.
```

Since the api calls are protected by the https ssl verification we need to supply the cluster cert which is located in the `kind-control-plane` container
```bash
docker ps -a 

f574b704ad2   kindest/node:v1.27.3   "/usr/local/bin/entr…"   3 hours ago   Up 3 hours   127.0.0.1:6443->6443/tcp   kind-control-plane
```
so now grab the `ca.crt` of the cluster from the pod and place it in the cert folder

```bash
docker cp kind-control-plane:/etc/kubernetes/pki/ca.crt cert/
```
With this you will get a `ca.crt` in the cert directory( which is omitted from this repo for obvious reason). 
```bash
curl --cacert cert/ca.crt  https://127.0.0.1:6443/version
{
  "major": "1",
  "minor": "27",
  "gitVersion": "v1.27.3",
  "gitCommit": "25b4e43193bcda6c7328a6d147b1fb73a33f1598",
  "gitTreeState": "clean",
  "buildDate": "2023-06-15T00:38:14Z",
  "goVersion": "go1.20.5",
  "compiler": "gc",
  "platform": "linux/arm64"
}%                                                                                                             
```