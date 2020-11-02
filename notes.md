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

## Certificate commands

openssl x509  -noout -text -in /etc/kubernetes/pki/apiserver.crt

## Security Commands

### To check access

```kubectl get pods --as dev-user```

### Check API access

```kubectl auth can-i create deployments --namespace dev```

```kubectl auth can-i list secrets --namespace dev --as dave```

### To know the user being used for a container

```kubectl exec ubuntu-sleeper -- whoami```

## General Commands

### To count resorces

```k get clusterroles --no-headers | wc -l```

### To get kubelet version

```kubelet --version```

### To load logs of service

```journalctl -u kubelet -f```

### systemctl commands

```systemctl status kubelet.service```
```systemctl restart kubelet```
```systemctl daemon-reload```

### kubelet

```ps aux | grep kubelet```

```/etc/systemd/system/kubelet.service.d/```

```whereis kubelet```

### For pod networking

```kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"```

### kubeconfig commands

```kubectl config view --kubeconfig=/root/my-kube-config```

```kubectl config view --kubeconfig=my-kube-config -o jsonpath="{.contexts[?(@.context.user=='aws-user')].name}"```


### Etcd backup

```ETCDCTL_API=3 etcdctl snapshot save /etc/etcd-snapshot.db \
--cacert /etc/kubernetes/pki/etcd/ca.crt \
--cert /etc/kubernetes/pki/etcd/server.crt \
--key /etc/kubernetes/pki/etcd/server.key
```

Need to pass cacert, cert and key

### Etcd restore

``` ETCDCTL_API=3 etcdctl snapshot restore /tmp/etcd-backup.db --data-dir /var/lib/etcd-backup ```

### systemd

We can see which components are controlled via systemd looking at /etc/systemd/system

### service account

kubectl create serviceaccount pvviewer

### To drain nodes

kubectl drain node01 --ignore-daemonsets
kubectl uncordon node01
kubectl cordon node01

### docker containers

A pod always has two at least two containers (if using docker).
The 3dffb59b81ac container is the main application
The ab2da239d3b5 is the pause conatiner and reserves the linux kernel network namespace and shares the IP with the other containers

### kubeadm join command

kubeadm token create --print-join-command

### static pod

The kubelet could also have a different manifests directory specified via parameter --pod-manifest-path which you could find out via ps aux | grep kubelet and checking the kubelet systemd config.

### service cidr

1. /etc/kubernetes/manifests/kube-apiserver.yaml : --service-cluster-ip-range
2. /etc/kubernetes/manifests/kube-controller-manager.yaml : --service-cluster-ip-range

### iptables

To view iptables associated with a service ssh into a node and run the command:
```iptables-save | grep service-name```

### find

find /etc/kubernetes/manifests -name etcd*
