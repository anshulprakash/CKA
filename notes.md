# Notes

## Cluster Information

### To get pod cidrs of nodes

- ```k describe node | less -p PodCIDR```
- ```k get node -o jsonpath="{range .items[*]}{.metadata.name} {.spec.podCIDR}{'\n'}"```

### To get service cidr

- ssh into master
- ```cat /etc/kubernetes/manifests/kube-apiserver.yaml | grep range```

### Which Networking (or CNI Plugin) is configured and where is its config file?

By default the kubelet looks into /etc/cni/net.d to discover the CNI plugins.

```find /etc/cni/net.d/```

