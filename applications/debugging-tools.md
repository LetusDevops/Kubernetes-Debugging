# Kubernetes Application Debugging Tools

This document outlines tools and techniques for debugging applications running on Kubernetes.

## Core Kubernetes Tools

### kubectl

The primary CLI tool for interacting with Kubernetes clusters.

**Useful Debugging Commands**:

1. **Port Forwarding**: Access applications without exposing them through a service.
   ```bash
   kubectl port-forward <pod-name> <local-port>:<pod-port>
   ```

2. **Exec**: Execute commands inside containers.
   ```bash
   kubectl exec -it <pod-name> -- /bin/sh
   ```

3. **Copy Files**: Copy files to/from containers.
   ```bash
   kubectl cp <pod-name>:/path/to/file /local/path
   ```

4. **Proxy**: Access the Kubernetes API server locally.
   ```bash
   kubectl proxy
   ```

5. **Top**: Monitor resource usage.
   ```bash
   kubectl top pods
   kubectl top nodes
   ```

### kube-state-metrics

Provides cluster-level metrics for Kubernetes objects.

**Installation**:
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/kube-state-metrics/master/examples/standard/cluster-role-binding.yaml
kubectl apply -f https://raw.githubusercontent.com/kubernetes/kube-state-metrics/master/examples/standard/cluster-role.yaml
kubectl apply -f https://raw.githubusercontent.com/kubernetes/kube-state-metrics/master/examples/standard/deployment.yaml
kubectl apply -f https://raw.githubusercontent.com/kubernetes/kube-state-metrics/master/examples/standard/service-account.yaml
kubectl apply -f https://raw.githubusercontent.com/kubernetes/kube-state-metrics/master/examples/standard/service.yaml
```

## Third-Party Tools

### k9s

Interactive CLI tool for managing Kubernetes clusters.

**Installation**:
```bash
# macOS
brew install k9s

# Linux
curl -sS https://raw.githubusercontent.com/derailed/k9s/master/install.sh | bash
```

**Usage**:
```bash
k9s
```

### stern

Multi-pod and container log tailing tool.

**Installation**:
```bash
# macOS
brew install stern

# Linux
curl -L -o stern https://github.com/stern/stern/releases/download/v1.22.0/stern_1.22.0_linux_amd64 && \
chmod +x stern && \
sudo mv stern /usr/local/bin/
```

**Usage**:
```bash
stern <pod-name-pattern>
```

### kubectx and kubens

Tools for switching between Kubernetes contexts and namespaces.

**Installation**:
```bash
# macOS
brew install kubectx

# Linux
git clone https://github.com/ahmetb/kubectx.git ~/.kubectx
export PATH=~/.kubectx:$PATH
```

**Usage**:
```bash
kubectx    # List or switch contexts
kubens     # List or switch namespaces
```

### Popeye

A Kubernetes cluster sanitizer and resource validator.

**Installation**:
```bash
# macOS
brew install popeye

# Linux
curl -L -o popeye https://github.com/derailed/popeye/releases/download/v0.10.0/popeye_0.10.0_Linux_x86_64.tar.gz && \
tar -xvf popeye_0.10.0_Linux_x86_64.tar.gz && \
chmod +x popeye && \
sudo mv popeye /usr/local/bin/
```

**Usage**:
```bash
popeye
```

## Advanced Debugging Techniques

### Ephemeral Debug Containers

Create debugging containers attached to a running pod (requires Kubernetes v1.18+).

```bash
kubectl debug -it <pod-name> --image=busybox --target=<container-name>
```

### Debugging with distroless images

For containers using distroless images without a shell:

```bash
kubectl debug <pod-name> -it --image=ubuntu
```

### Interactive Visual Debuggers

Use `kubectl` with IDEs that support Kubernetes debugging:

1. **VS Code with the Kubernetes extension**: Set breakpoints and debug directly from the IDE.
2. **JetBrains IDEs (IntelliJ, etc.)**: Use the Kubernetes plugin for remote debugging.

## Monitoring Tools

### Prometheus and Grafana

Monitor application metrics with Prometheus and visualize with Grafana.

**Installation (using Helm)**:
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm install prometheus prometheus-community/kube-prometheus-stack
```

### Jaeger

Distributed tracing for microservices.

**Installation (using Helm)**:
```bash
helm repo add jaegertracing https://jaegertracing.github.io/helm-charts
helm install jaeger jaegertracing/jaeger
```

## Tips & Tricks

1. **Use Init Containers for Debugging**:
   Add an init container to verify prerequisites before your app starts.

2. **Create a Debugging Deployment**:
   Deploy troubleshooting tools in the same namespace for networking tests.
   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: debug-tools
   spec:
     selector:
       matchLabels:
         app: debug-tools
     template:
       metadata:
         labels:
           app: debug-tools
       spec:
         containers:
         - name: debug-tools
           image: nicolaka/netshoot
           command: ['sleep', '3600']
   ```

3. **Use Labels for Debugging**:
   Add temporary labels to filter resources during debugging.
   ```bash
   kubectl label pod <pod-name> debug=true
   kubectl get pods -l debug=true
   ```