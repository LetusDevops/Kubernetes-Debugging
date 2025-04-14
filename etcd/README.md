# Etcd Debugging

This folder contains information and tools for debugging the distributed key-value store (etcd).

## Debugging Steps

1. **Check Etcd Logs**:
   - Use `kubectl logs -n kube-system <etcd-pod-name>` to view logs.
   - Look for errors related to data storage or cluster communication.

2. **Verify Etcd Health**:
   - Use `etcdctl` to check the health of the etcd cluster.
   - Example: `etcdctl --endpoints=<etcd-endpoints> endpoint health`.

3. **Inspect Data**:
   - Use `etcdctl` to inspect keys and values stored in etcd.
   - Example: `etcdctl --endpoints=<etcd-endpoints> get / --prefix`.

4. **Check Disk Usage**:
   - Ensure that the disk hosting etcd data has sufficient free space.

5. **Inspect Configuration**:
   - Review the etcd configuration file (e.g., `/etc/kubernetes/manifests/etcd.yaml`).

## Tools

- `kubectl`
- `etcdctl`
- Disk monitoring tools like `df` or `du`.

## Example Issue

**Problem**: Etcd cluster is unhealthy.

**Solution**:
1. Check etcd logs: `kubectl logs -n kube-system <etcd-pod-name>`.
2. Verify etcd health: `etcdctl --endpoints=<etcd-endpoints> endpoint health`.
3. Inspect disk usage: `df -h` or `du -sh /var/lib/etcd`.