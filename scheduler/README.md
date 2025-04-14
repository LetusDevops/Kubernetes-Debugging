# Scheduler Debugging

This folder contains information and tools for debugging the Kubernetes scheduler.

## Debugging Steps

1. **Check Scheduler Logs**:
   - Use `kubectl logs -n kube-system <scheduler-pod-name>` to view logs.
   - Look for errors related to scheduling decisions or resource constraints.

2. **Verify Scheduler Health**:
   - Run `kubectl get componentstatuses` to check the health of the scheduler.

3. **Inspect Events**:
   - Use `kubectl get events --all-namespaces` to identify scheduling-related issues.

4. **Simulate Scheduling**:
   - Use `kubectl describe pod <pod-name>` to understand why a pod is not being scheduled.

5. **Inspect Configuration**:
   - Review the scheduler configuration file (e.g., `/etc/kubernetes/manifests/kube-scheduler.yaml`).

## Tools

- `kubectl`
- Log analysis tools like `stern` or `k9s`.

## Example Issue

**Problem**: Pods are stuck in `Pending` state.

**Solution**:
1. Check scheduler logs: `kubectl logs -n kube-system <scheduler-pod-name>`.
2. Inspect pod events: `kubectl describe pod <pod-name>`.
3. Verify node availability: `kubectl get nodes` and check for `Ready` status.