# Networking Debugging

This folder contains information and tools for debugging Kubernetes networking issues.

## Debugging Steps

1. **Check Pod Networking**:
   - Use `kubectl get pods -o wide` to view pod IPs.
   - Use `ping` or `curl` to test connectivity between pods.

2. **Inspect Network Policies**:
   - Use `kubectl get networkpolicy` to list network policies.
   - Verify that policies are not blocking traffic.

3. **Verify CNI Plugin**:
   - Check logs of the CNI plugin pods (e.g., `kubectl logs -n kube-system <cni-pod-name>`).

4. **Inspect Node Networking**:
   - Use `ifconfig` or `ip a` on nodes to check network interfaces.
   - Use `iptables` to inspect firewall rules.

5. **Inspect Configuration**:
   - Review the CNI plugin configuration file (e.g., `/etc/cni/net.d/`).

## Tools

- `kubectl`
- Network tools like `ping`, `curl`, `traceroute`.
- Node tools like `ifconfig`, `ip a`, `iptables`.

## Example Issue

**Problem**: Pods cannot communicate with each other.

**Solution**:
1. Check pod IPs: `kubectl get pods -o wide`.
2. Test connectivity: `ping <pod-ip>` or `curl http://<pod-ip>:<port>`.
3. Inspect network policies: `kubectl get networkpolicy`.
4. Verify CNI plugin logs: `kubectl logs -n kube-system <cni-pod-name>`.