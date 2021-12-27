# RTSP Network Tools Docker Image

Debian with basic network tools included.


## Usage

In order to use image from GitHub Container Registry instead of Docker Hub, you can replace `rtsp/net-tools` with `ghcr.io/rtsp/docker-net-tools` anywhere in the instruction below.

### Pull Image

```ShellSession
docker pull rtsp/net-tools
```

### Interactive Mode

```ShellSession
docker run --rm -it rtsp/net-tools bash
```

### Run a Specific Command

```ShellSession
docker run --rm rtsp/net-tools ping -c 3 172.17.0.1
```

### Run as Daemon

```ShellSession
docker run -d --name net-tools rtsp/net-tools
```

```ShellSession
docker exec net-tools ping -c 3 172.17.0.1

docker exec -it net-tools bash
```

### Run as Kubernetes Pod

```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: net-tools
spec:
  containers:
  - name: net-tools
    image: rtsp/net-tools:latest
```

```ShellSession
kubectl exec net-tools -- ping -c 3 10.233.0.1

kubectl exec -it net-tools -- bash
```

### Run as Kubernetes DaemonSet

This manifests will deploy Pods in all nodes (including control-plane). Useful for network reachability debugging.

```yaml
---
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: net-tools
spec:
  selector:
    matchLabels:
      name: net-tools
  template:
    metadata:
      labels:
        name: net-tools
    spec:
      tolerations:
      - key: node-role.kubernetes.io/master
        operator: Exists
        effect: NoSchedule
      containers:
        - name: net-tools
          image: rtsp/net-tools:latest
          imagePullPolicy: IfNotPresent
      terminationGracePeriodSeconds: 30
```

Retrieve Pods Name

```ShellSession
kubectl get pods -l name=net-tools -o wide
```

Output (Trimmed)
```
NAME              READY   STATUS    AGE     IP             NODE
net-tools-j7j97   1/1     Running   4d5h    10.233.69.40   k8s-w3
net-tools-kfx7h   1/1     Running   4d16h   10.233.66.10   k8s-m3
net-tools-krttb   1/1     Running   4d16h   10.233.68.43   k8s-w1
net-tools-l8tjd   1/1     Running   4d16h   10.233.67.36   k8s-w2
net-tools-q48n6   1/1     Running   4d16h   10.233.65.7    k8s-m2
net-tools-vw45v   1/1     Running   4d16h   10.233.64.11   k8s-m1
```

Example: Check that `k8s-m2` node (`net-tools-q48n6`) able to reach a pod in `k8s-m1` node (`10.233.64.11`).

```ShellSession
kubectl exec net-tools-q48n6 -- ping -c 3 10.233.64.11
```

or just run it interactively

```ShellSession
kubectl exec -it net-tools-q48n6 -- bash
```

or if you don't mind which Pod to use

```ShellSession
kubectl exec -it daemonset/net-tools -- bash
```


## Links

### Packages

- Docker Hub: [rtsp/net-tools](https://hub.docker.com/r/rtsp/net-tools/)
- GitHub: [ghcr.io/rtsp/docker-net-tools](https://github.com/rtsp/docker-net-tools/pkgs/container/docker-net-tools)

### Source Code

- [rtsp/docker-net-tools](https://github.com/rtsp/docker-net-tools)
