# Storage Debugging

This folder contains information and tools for debugging Kubernetes storage issues.

## Debugging Steps

1. **Check PersistentVolume (PV) and PersistentVolumeClaim (PVC) Status**:
   - Use `kubectl get pv` and `kubectl get pvc` to check the status of volumes and claims.

2. **Inspect Events**:
   - Use `kubectl describe pvc <pvc-name>` to view events related to the PVC.

3. **Verify Storage Class**:
   - Use `kubectl get storageclass` to list available storage classes.
   - Ensure the correct storage class is being used.

4. **Inspect Node Storage**:
   - Use tools like `df` or `lsblk` on nodes to check disk usage and availability.

5. **Inspect Configuration**:
   - Review the storage configuration in the cluster.

## Tools

- `kubectl`
- Node tools like `df`, `lsblk`, or `mount`.

## Example Issue

**Problem**: PVC is stuck in `Pending` state.

**Solution**:
1. Check PVC events: `kubectl describe pvc <pvc-name>`.
2. Verify storage class: `kubectl get storageclass`.
3. Inspect node storage: `df -h` or `lsblk`.