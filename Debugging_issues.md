# 🐞 Debugging and Troubleshooting Log

This document records key issues encountered during the initial setup of the local Kubernetes cluster using Minikube and Docker on a Windows 11 machine, along with the precise fixes applied.

---

## Issue 1: Docker Engine Connection Failure

### 🔍 Error Message

During the first attempt to start Minikube, the following error indicated a lack of connectivity to the Docker daemon.

X Exiting due to PROVIDER_DOCKER_VERSION_EXIT_1: "docker version --format <no value>-<no value>:<no value>" exit status 1: error during connect: Get "http://%2F%2F.%2Fpipe%2FdockerDesktopLinuxEngine/v1.48/version": open //./pipe/dockerDesktopLinuxEngine: The system cannot find the file specified.

### ✅ Resolution

This error typically means the **Docker Desktop application was not running or fully initialized** before Minikube was started.

1.  Ensured **Docker Desktop was launched** and showed the green indicator (running status).
2.  Executed the `minikube start` command again:

    ```bash
    minikube start --driver=docker
    ```

---

## Issue 2: Corrupted `kubectl.exe` Binary

### 🔍 Error Message

After the cluster successfully started (`minikube` command completed), the final step failed with an error indicating the `kubectl` executable was unusable.

E1027 14:05:28.360503 23868 start.go:298] kubectl info: exec: fork/exec C:\WINDOWS\System32\kubectl.exe: The file or directory is corrupted and unreadable.


### ✅ Resolution

This required administrative privileges to remove the bad file and reinstall the utility.

1.  Opened **PowerShell as Administrator**.
2.  Forced the removal of the corrupted file:
    ```powershell
    Remove-Item C:\WINDOWS\System32\kubectl.exe -Force
    ```
3.  Reinstalled/verified the installation of `kubectl` using `winget`:
    ```powershell
    winget install Kubernetes.kubectl
    ```
4.  Verified the fix:
    ```bash
    kubectl version --client
    kubectl get nodes # Confirmed node was "Ready"
    ```

---

## Issue 3: Inaccessible Web Application URL

### 🔍 Symptom

The Service was successfully deployed, and `minikube service my-nginx-service --url` returned a local URL (e.g., `http://127.0.0.1:61582`), but the browser connection timed out or failed to open the Nginx page.

### ✅ Resolution

When using the **Docker driver on Windows**, NodePort services often require the Minikube tunnel to correctly route traffic from the host operating system to the Docker container network.

1.  Opened a **separate, dedicated PowerShell window**.
2.  Executed and kept running the Minikube tunnel command:
    ```bash
    minikube tunnel
    ```
3.  In the original terminal, re-ran the URL command, and the application became accessible in the browser.