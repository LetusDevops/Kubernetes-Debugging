# Debugging Applications on Kubernetes

This folder contains information and tools for debugging applications running on Kubernetes.

## Pod Logs

### Basic Log Commands

1. **View Pod Logs**:
   ```bash
   kubectl logs <pod-name>
   ```

2. **Follow Pod Logs in Real-Time**:
   ```bash
   kubectl logs -f <pod-name>
   ```

3. **View Logs from Previous Pod Instance**:
   ```bash
   kubectl logs <pod-name> --previous
   ```

4. **View Logs from a Specific Container in a Pod**:
   ```bash
   kubectl logs <pod-name> -c <container-name>
   ```

5. **View Logs with Timestamps**:
   ```bash
   kubectl logs <pod-name> --timestamps
   ```

6. **View a Limited Number of Lines**:
   ```bash
   kubectl logs <pod-name> --tail=100
   ```

7. **View Logs Since a Specific Time**:
   ```bash
   kubectl logs <pod-name> --since=1h
   ```

### Advanced Log Tools

1. **Stern**: A multi-pod and container log tailing tool.
   ```bash
   stern <pod-name-pattern>
   ```

2. **k9s**: Interactive Kubernetes CLI with log viewing capabilities.
   ```bash
   k9s
   ```

## Pod Events

### Viewing Events

1. **View All Events in the Cluster**:
   ```bash
   kubectl get events --sort-by=.metadata.creationTimestamp
   ```

2. **View Events for a Specific Pod**:
   ```bash
   kubectl describe pod <pod-name>
   ```

3. **View Events for a Specific Namespace**:
   ```bash
   kubectl get events -n <namespace>
   ```

### Common Pod Events and Their Meanings

| Event Type | Event Reason | Description | Troubleshooting |
|------------|-------------|-------------|----------------|
| Normal | Scheduled | The pod has been scheduled to a node | N/A |
| Normal | Pulled | Container image has been successfully pulled | N/A |
| Normal | Created | Container has been created | N/A |
| Normal | Started | Container has been started | N/A |
| Warning | Failed | Pod failed to be scheduled | Check node resources and pod requests/limits |
| Warning | FailedScheduling | The scheduler cannot schedule the pod | Check node resources, taints/tolerations, or node affinity |
| Warning | Failed | Error creating container | Check container configuration and image name |
| Warning | BackOff | Container is repeatedly crashing | Check application logs and startup configuration |
| Warning | ImagePullBackOff | Failed to pull the container image | Check image name, registry credentials, and network connectivity |
| Warning | CrashLoopBackOff | Container is repeatedly crashing | Check application logs and startup configuration |
| Warning | Unhealthy | Readiness or liveness probe failed | Check probe configuration and application health |
| Normal | Killing | Container is being terminated | N/A |
| Warning | Evicted | Pod was evicted due to node issues | Check node resources and pod resource requirements |
| Warning | FailedMount | Failed to mount a volume | Check volume configuration and availability |
| Normal | Preempting | Pod is being preempted by a higher-priority pod | N/A |
| Warning | ExceededGracePeriod | Container did not terminate within grace period | Check if container responds to termination signals |

## Debugging Steps

1. **Check Pod Status**:
   ```bash
   kubectl get pods
   ```

2. **Describe the Pod for Events and Details**:
   ```bash
   kubectl describe pod <pod-name>
   ```

3. **Check Container Logs**:
   ```bash
   kubectl logs <pod-name> -c <container-name>
   ```

4. **Check Resource Usage**:
   ```bash
   kubectl top pod <pod-name>
   ```

5. **Exec into the Pod for Troubleshooting**:
   ```bash
   kubectl exec -it <pod-name> -- /bin/sh
   ```

## Example Issue

**Problem**: Application pod is in CrashLoopBackOff state.

**Solution**:
1. Check pod events: `kubectl describe pod <pod-name>`.
2. Check container logs: `kubectl logs <pod-name> --previous`.
3. Verify configuration: Check environment variables, volume mounts, and resource requests/limits.
4. Test application locally: Run the container locally to verify if the issue is application-specific.