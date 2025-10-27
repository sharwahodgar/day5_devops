minikube start --driver=docker

3. **Critical Setup Fix (Optional but included in history):** During initial setup, a corrupted `kubectl.exe` was encountered. This was fixed by running the following in an **Administrator PowerShell**:

   <comment-tag id="3">


Remove-Item C:\WINDOWS\System32\kubectl.exe -Force winget install Kubernetes.kubectl

### B. Deployment Steps

Run these commands from the root directory of this repository (where the YAML files are located).

| **Step** | **Command** | **Verification** | 
| **1. Deploy Application** | `kubectl apply -f deployment.yaml` | `kubectl get deployments` | 
| **2. Verify Pods** | `kubectl get pods` | Wait until all 3 Pods show `STATUS: Running` and `READY: 1/1`. | 
| **3. Deploy Service** | `kubectl apply -f service.yaml` | `kubectl get services` | 
| **4. Scale Deployment** | `kubectl scale deployment my-nginx-deployment --replicas=5` | `kubectl get pods` (5 Pods should appear). | 
| **5. Access Application** | `minikube service my-nginx-service --url` | Copy the resulting URL (e.g., `http://127.0.0.1:61582`) into a web browser. | 
| **6. Inspect Details (Debugging)** | `kubectl describe deployment my-nginx-deployment` | Review the historical scaling events and current configuration. | 

## 3. Configuration Files

### `deployment.yaml`

<comment-tag id="4">This file defines a desired state of 3 running Nginx web servers.


NodePort type exposes the service on a port open on the Minikube host machine
type: NodePort ports: - protocol: TCP port: 80 targetPort: 80


## 4. Cleanup

To stop and tear down the cluster environment when finished:

```
# Delete the Deployment and Service resources
kubectl delete deployment my-nginx-deployment
kubectl delete service my-nginx-service

# Stop the Minikube VM/Container
minikube stop

# Delete the Minikube cluster entirely (recommended after a session)
minikube delete

```

**Author Note:** This project successfully demonstrated the core Kubernetes workflow from installation to application management.
