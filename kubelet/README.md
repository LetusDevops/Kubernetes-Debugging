# Kubelet Debugging

This folder contains information and tools for debugging the Kubernetes node agent (kubelet).

## Debugging Steps

1. **Check Kubelet Logs**:
   - Use `journalctl -u kubelet` to view logs on the node.
   - Look for errors related to pod creation, health checks, or resource usage.

2. **Verify Node Health**:
   - Run `kubectl get nodes` to check the status of nodes.
   - Use `kubectl describe node <node-name>` for detailed information.

3. **Inspect Pod Status**:
   - Use `kubectl get pods -o wide` to check pod status and node assignment.
   - Use `kubectl describe pod <pod-name>` for more details.

4. **Check Resource Usage**:
   - Use tools like `top` or `htop` on the node to monitor CPU and memory usage.

5. **Inspect Configuration**:
   - Review the kubelet configuration file (e.g., `/var/lib/kubelet/config.yaml`).

## Tools

- `kubectl`
- `journalctl`
- Node monitoring tools like `top`, `htop`, or `nmon`.

## Example Issue

**Problem**: Pods are not starting on a node.

**Solution**:
1. Check kubelet logs: `journalctl -u kubelet`.
2. Verify node status: `kubectl get nodes`.
3. Inspect pod events: `kubectl describe pod <pod-name>`.
4. Check resource availability on the node: `top` or `htop`.