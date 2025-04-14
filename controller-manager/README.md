# Controller Manager Debugging

This folder contains information and tools for debugging the Kubernetes controller manager.

## Debugging Steps

1. **Check Controller Manager Logs**:
   - Use `kubectl logs -n kube-system <controller-manager-pod-name>` to view logs.
   - Look for errors related to resource reconciliation or controller loops.

2. **Verify Controller Health**:
   - Run `kubectl get componentstatuses` to check the health of the controller manager.

3. **Inspect Events**:
   - Use `kubectl get events --all-namespaces` to identify issues managed by controllers.

4. **Inspect Configuration**:
   - Review the controller manager configuration file (e.g., `/etc/kubernetes/manifests/kube-controller-manager.yaml`).

## Tools

- `kubectl`
- Log analysis tools like `stern` or `k9s`.

## Example Issue

**Problem**: PersistentVolumeClaims (PVCs) are not being bound.

**Solution**:
1. Check controller manager logs: `kubectl logs -n kube-system <controller-manager-pod-name>`.
2. Inspect PVC events: `kubectl describe pvc <pvc-name>`.
3. Verify storage class configuration: `kubectl get storageclass`.