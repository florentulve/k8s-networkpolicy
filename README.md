# K8s Network Policy Example


Bootstrap

```sh
multipass lanch --name k3s
multipass shell
curl -sfL https://get.k3s.io | sh -
```

Copy /etc/rancher/k3s/k3s.yml to ~/.kube/config

```sh
kubectl apply -f .
```

Obtain a shell within sandbox pod ns1 or spwn one

```sh
kubectl run test-$RANDOM --namespace=ns2 --rm -i -t --image=alpine -- sh
```

```sh
curl -v http://sevice1.ns1
```

Works ✅

```sh
curl -v http://sevice1.ns1
```

Works ✅

By Default no isolation ☢️

Deny all traffic:

[https://github.com/ahmetb/kubernetes-network-policy-recipes/blob/master/04-deny-traffic-from-other-namespaces.md](https://github.com/ahmetb/kubernetes-network-policy-recipes/blob/master/04-deny-traffic-from-other-namespaces.md)

```yaml
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  namespace: default
  name: deny-from-other-namespaces
spec:
  podSelector:
    matchLabels:
  ingress:
  - from:
    - podSelector: {}
```

```sh
kubectl apply -f ./policies
```

Obtain a shell within sandbox pod ns1 or spawn one with

```sh
kubectl run test-$RANDOM --namespace=ns1 --rm -i -t --image=alpine -- sh
```

```sh
curl -v http://sevice1.ns1
```

Works ✅

```sh
curl -v http://sevice1.ns1
```

Doest work ⛔️
