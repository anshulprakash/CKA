# Notes

Some important points based on mock tests

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

## Cluster Event Logging

### To show the latest events in the whole cluster, ordered by time

```kubectl get events -A --sort-by=.metadata.creationTimestamp```

## Namespaces and Api Resources

### To get names of all namespaced Kubernetes resources

```k api-resources --namespaced -o name```

## Docker commands

### To list all containers associated with a pod

```docker ps | grep <<pod-name>>```

### To write docker container logs to a file

```"docker logs <<container_id>>" &> pod-container.log```
The &> in above command redirects both the standard output and standard error