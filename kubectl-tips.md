# Kubectl Faster

## Setup

- ```alias k=kubectl```
- ```export do="--dry-run=client -o yaml"```

## kubectl run

kubectl run will always create a Pod and the --restart=X sets the Pod Restart Policy field.

## Create Pod YAML

### Simple

```k run pod1 --image=busybox $do```

### Complex

```k run pod1 $do --image=busybox --requests "cpu=100m,memory=256Mi" --limits "cpu=200m,memory=512Mi" --command -- sh -c "sleep 1d"```

## Create Deployment YAML

### Simple

```k create deploy deploy1 --image=nginx $do ```

### Complex

```
#Generate deployment into a file
# we generate a deployment into a file
k create deploy deploy1 -oyaml --image=busybox --dry-run=client > deploy1.yaml
# generate pod yaml and append to same file
k run deploy1 \
    -oyaml \
    --dry-run=client \
    --image=busybox \
    --requests "cpu=100m,memory=256Mi" \
    --limits "cpu=200m,memory=512Mi" \
    --command \
    -- sh -c "sleep 1d" >> deploy1.yaml
# now edit deploy1.yaml, copy everything you need from the pod yaml
vim deploy1.yaml
```
