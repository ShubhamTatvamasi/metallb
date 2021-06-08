# metallb

Install Metal LB:
```bash
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.10.0/manifests/namespace.yaml
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.10.0/manifests/metallb.yaml
```
> Source: https://metallb.universe.tf/installation/#installation-by-manifest

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

CIDR to IP Range:
https://www.ipaddressguide.com/cidr
