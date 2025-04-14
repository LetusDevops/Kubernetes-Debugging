# Common Application Errors on Kubernetes

This document outlines common errors encountered when running applications on Kubernetes and how to resolve them.

## Container Startup Failures

### Issue: CrashLoopBackOff

**Symptoms**: 
- Pod status is `CrashLoopBackOff`
- Container repeatedly restarts

**Common Causes**:
1. Application error on startup
2. Missing configuration or environment variables
3. Insufficient resources
4. Invalid command or arguments

**Debugging Steps**:
1. View previous container logs:
   ```bash
   kubectl logs <pod-name> --previous
   ```
2. Check container configuration:
   ```bash
   kubectl describe pod <pod-name>
   ```
3. Verify environment variables and volume mounts

### Issue: ImagePullBackOff

**Symptoms**:
- Pod status is `ImagePullBackOff` or `ErrImagePull`

**Common Causes**:
1. Image does not exist
2. Registry authentication issues
3. Network connectivity problems

**Debugging Steps**:
1. Verify image name and tag:
   ```bash
   kubectl describe pod <pod-name>
   ```
2. Check registry credentials:
   ```bash
   kubectl get secret <registry-secret> -o yaml
   ```
3. Test image pull manually on a node:
   ```bash
   docker pull <image>
   ```

## Resource Issues

### Issue: OOMKilled

**Symptoms**:
- Container is terminated with reason `OOMKilled`
- Application logs may show memory-related errors

**Common Causes**:
1. Memory leak in application
2. Insufficient memory limits
3. JVM or other runtime configured incorrectly

**Debugging Steps**:
1. Check memory usage:
   ```bash
   kubectl top pod <pod-name>
   ```
2. Increase memory limits in deployment YAML:
   ```yaml
   resources:
     limits:
       memory: "512Mi"
     requests:
       memory: "256Mi"
   ```
3. Analyze application for memory leaks

### Issue: CPU Throttling

**Symptoms**:
- Application performance is slow
- High CPU usage

**Debugging Steps**:
1. Check CPU usage:
   ```bash
   kubectl top pod <pod-name>
   ```
2. Adjust CPU limits in deployment YAML

## Networking Issues

### Issue: Connection Refused

**Symptoms**:
- Application logs show connection refused errors
- Services cannot communicate

**Common Causes**:
1. Service name resolution issues
2. Application not listening on expected port
3. Network policies blocking traffic

**Debugging Steps**:
1. Verify service is running:
   ```bash
   kubectl get svc <service-name>
   ```
2. Check endpoints:
   ```bash
   kubectl get endpoints <service-name>
   ```
3. Test connectivity from within a pod:
   ```bash
   kubectl exec -it <pod-name> -- curl <service-name>:<port>
   ```
4. Check network policies:
   ```bash
   kubectl get networkpolicy
   ```

## Authorization Issues

### Issue: Permission Denied

**Symptoms**:
- Pod cannot access resources
- Logs show authorization errors

**Common Causes**:
1. Missing or incorrect RBAC configuration
2. Service account lacks necessary permissions

**Debugging Steps**:
1. Check pod's service account:
   ```bash
   kubectl describe pod <pod-name>
   ```
2. Verify service account permissions:
   ```bash
   kubectl auth can-i --list --as=system:serviceaccount:<namespace>:<serviceaccount>
   ```
3. Update RBAC configuration as needed