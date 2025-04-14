# API Server Debugging

This folder contains information and tools for debugging the Kubernetes API server.

## Debugging Steps

1. **Check API Server Logs**:
   - Use `kubectl logs -n kube-system <api-server-pod-name>` to view logs.
   - Look for errors or warnings related to authentication, authorization, or resource requests.

2. **Verify API Server Health**:
   - Run `kubectl get componentstatuses` to check the health of the API server.

3. **Test API Endpoints**:
   - Use `curl` or tools like Postman to test API endpoints.
   - Example: `curl -k https://<api-server-ip>:6443/healthz`.

4. **Check Certificates**:
   - Ensure that the certificates used by the API server are valid and not expired.
   - Example: `openssl x509 -in <certificate-file> -text -noout`.

5. **Inspect Configuration**:
   - Review the API server configuration file (e.g., `/etc/kubernetes/manifests/kube-apiserver.yaml`).

## Tools

- `kubectl`
- `curl`
- `openssl`
- Log analysis tools like `stern` or `k9s`.

## Example Issue

**Problem**: API server is not responding.

**Solution**:
1. Check if the API server pod is running: `kubectl get pods -n kube-system`.
2. Inspect logs for errors: `kubectl logs -n kube-system <api-server-pod-name>`.
3. Verify network connectivity to the API server: `ping <api-server-ip>` or `telnet <api-server-ip> 6443`.