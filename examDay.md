# On day of Exam

1. turn kubectl autocompletion on from kubernetes.io (this includes alias for kubectl) https://kubernetes.io/docs/reference/kubectl/cheatsheet/#kubectl-autocomplete
2. export do="--dry-run=client -o yaml"
3. export ks="kube-system"
4. Edit .vimrc

```
set tabstop=2
set shiftwidth=2
set expandtab
```

5. alias kns="kubectl get pods -n kube-system"