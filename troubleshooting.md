# Troubleshooting

## Application failure

1. Check if service names are correct
2. Check endpoints are correct (if endpoints are incorrect then check labels)
3. Check port mapping of services
4. Check credentials used are correct in definiton files

## Control Plane failure

1. Pod in pending state - check kube-scheduler pod
2. Deployments don't scale - check kube-controller

## Worker node failure

1. Node not in ready state - check kubelet service

- restart kubelet service
- check service logs using journalctl
- check configuration
- check api server address in config if connection timeout errors

## Network

1. Check if pod networking plugin is installed
2. Check kube-proxy po - check how its ds is defined and configured
3. Check endpoints in kube-system ns

If the kube-dns service is not working as expected. The first thing to check is if the service has a valid endpoint? Does it point to the kube-dbs/core-dns ?

Run: kubectl -n kube-system get ep kube-dns

If there are no endpoints for the service, inspect the service and make sure it uses the correct selectors and ports.

Run: kubectl -n kube-system descrive svc kube-dns

4. Check network policies (there may be default deny)
