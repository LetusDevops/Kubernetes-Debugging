# What Happens When You Run kubectl Behind the Scenes

This document provides a detailed explanation of what happens when you execute a `kubectl` command. Understanding this flow is important for debugging Kubernetes issues effectively.

## 1. Command Execution Flow

When you run a kubectl command (e.g., `kubectl get pods`), the following sequence of events occurs:

### 1.1 Client-Side Processing

1. **Command Parsing**: 
   - kubectl parses the command and flags
   - Validates syntax and command structure
   - Loads configuration (from kubeconfig file)

2. **Authentication Preparation**:
   - Reads credentials from kubeconfig
   - Prepares authentication headers/tokens
   - Sets up client certificates if required

3. **API Request Formation**:
   - Constructs the appropriate HTTP request
   - Adds necessary headers and authentication information
   - Formats query parameters and resource paths

### 1.2 Client to API Server Communication

4. **Connection Establishment**:
   - Creates a secure connection to the Kubernetes API server
   - Performs TLS handshake with the server
   - Validates server certificates

5. **Request Transmission**:
   - Sends the HTTP request to the API server
   - Includes authentication tokens/certificates
   - Sets proper content types and accepts headers

### 1.3 API Server Processing

6. **Authentication**:
   - API server authenticates the request using configured authentication plugins
   - Verifies tokens, certificates, or other credentials
   - Establishes user identity

7. **Authorization**:
   - Checks if the authenticated user has permissions for the requested operation
   - Uses RBAC rules to determine access rights
   - Denies the request if permissions are insufficient

8. **Admission Control**:
   - Runs the request through admission controllers
   - Validates the request against policies
   - May modify the request based on mutation webhooks

9. **Resource Validation**:
   - Validates the requested operation against the resource schema
   - Ensures required fields are present
   - Checks field types and constraints

### 1.4 Backend Processing

10. **etcd Interaction**:
    - For read operations: API server retrieves data from etcd
    - For write operations: API server writes data to etcd
    - Handles optimistic concurrency control with resource versions

11. **Resource Processing**:
    - For create/update operations: Processes defaults, validates rules
    - For delete operations: Handles finalizers and cascading deletion
    - For watch operations: Sets up watch streams

### 1.5 Response Handling

12. **Response Generation**:
    - API server formats the response (typically JSON)
    - Includes resource version and other metadata
    - Handles pagination for list responses

13. **Response Transmission**:
    - Sends the response back to kubectl
    - Includes appropriate status codes
    - Compresses data if supported

### 1.6 Client-Side Display

14. **Output Processing**:
    - kubectl receives and processes the response
    - Formats data according to output flags (JSON, YAML, etc.)
    - Applies any client-side filtering

15. **Display to User**:
    - Renders the output to the console
    - Formats tables, colors, or other display elements
    - Shows error messages if applicable

## 2. Common Debug Points

When debugging kubectl commands, consider these common failure points:

- **Authentication Issues**: Certificate problems, token expiration, misconfigured kubeconfig
- **Authorization Issues**: RBAC permissions, role bindings
- **Network Issues**: Firewalls, proxies, DNS resolution
- **API Server Health**: Overloaded API server, etcd connectivity
- **Resource Conflicts**: Optimistic locking failures, resource version mismatches

## 3. Debugging Tips

### 3.1 Verbose Output

Use `-v` flag with increasing levels to get more detailed output:

```sh
# Increasing verbosity levels
kubectl get pods -v=1
kubectl get pods -v=6
kubectl get pods -v=10
```

### 3.2 Examining API Requests

To see the exact API requests being made:

```sh
kubectl get pods -v=8
```

### 3.3 Using curl for Direct API Access

Bypass kubectl and interact directly with the API:

```sh
# Get the API server URL and token
SERVER=$(kubectl config view -o jsonpath='{.clusters[0].cluster.server}')
TOKEN=$(kubectl get secret $(kubectl get serviceaccount default -o jsonpath='{.secrets[0].name}') -o jsonpath='{.data.token}' | base64 --decode)

# Make a direct API call
curl -X GET $SERVER/api/v1/namespaces/default/pods --header "Authorization: Bearer $TOKEN" --insecure
```

### 3.4 Examining the API Server Logs

Check API server logs for detailed server-side information:

```sh
# If using Minikube
minikube ssh
sudo journalctl -u kubelet

# If using kubeadm
ssh <master-node>
sudo journalctl -u kube-apiserver
```

## 4. Advanced Topics

### 4.1 Custom Resource Definitions (CRDs)

The flow for custom resources is similar but includes:
- API server extension via CRDs
- Custom controllers watching for these resources
- Potentially custom admission webhooks

### 4.2 Aggregated API Servers

Some resources might be handled by aggregated API servers, which:
- Run as separate services
- Are discovered via the main API server
- Handle specific resource types

### 4.3 Server-Side Apply

With `kubectl apply --server-side`, the flow changes to:
- Send the complete object to the server
- Let the server handle the merge strategy
- Reduce client-side processing

## 5. References

- [Kubernetes API Documentation](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.25/)
- [kubectl Book](https://kubectl.docs.kubernetes.io/)
- [Kubernetes Internals Architecture](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/architecture/architecture.md)