# metallb

Add helm repo:
```bash
helm repo add metallb https://metallb.github.io/metallb
helm repo update

helm upgrade -i metallb metallb/metallb \
  --create-namespace \
  --namespace metallb-system
```

Create IP Pool Custome Resource:
```bash
kubectl create -f - << EOF
apiVersion: metallb.io/v1beta1
kind: IPAddressPool
metadata:
  namespace: metallb-system
  name: ip-pool
spec:
  addresses:
  - $(kubectl get nodes -o jsonpath='{ $.items[*].status.addresses[?(@.type=="ExternalIP")].address }')/32
EOF
```


### OLD

Setup IP range. Giving only 1 IP which is of the node IP
```bash
kubectl create -f - << EOF
apiVersion: v1
kind: ConfigMap
metadata:
  namespace: metallb-system
  name: config
data:
  config: |
    address-pools:
    - name: address-pool-1
      protocol: layer2
      addresses:
      - $(kubectl get nodes -o jsonpath='{ $.items[*].status.addresses[?(@.type=="ExternalIP")].address }')/32
EOF
```

---

Install Metal LB:
```bash
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.11.0/manifests/namespace.yaml
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.11.0/manifests/metallb.yaml
```
> Source: https://metallb.universe.tf/installation/#installation-by-manifest

CIDR to IP Range:
https://www.ipaddressguide.com/cidr
