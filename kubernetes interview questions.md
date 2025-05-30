#Some common errors in #Kubernetes #troubleshooting 

1. Pod in CrashLoopBackOff:
 - Check the pod logs using kubectl logs.
 - Ensure the required dependencies are available.

2. ImagePullBackOff:
 - Verify the image name and availability.
 - Check image pull secrets and registry authentication.

3. Invalid ClusterIP or Service Port:
 - Ensure correct service and port configurations.
 - Verify network policies and firewall rules.

4. NodeNotReady: 
 - Check the node's status with kubectl get nodes.
 - Examine the kubelet logs on the node.

5. Insufficient Resources:
 - Confirm resource requests and limits in pod specifications.
 - Use kubectl describe to analyze resource allocations.

6. CrashLoopBackOff for Init Containers:
 - Inspect init container logs and configurations.
 - Ensure init container dependencies are satisfied.

7. ConfigMap or Secret Mounting Issues:
 - Verify ConfigMap/Secret existence and correctness.
 - Check pod configuration for volume mounts.

8. Pod stuck in Pending state:
 - Investigate resource constraints on the nodes.
 - Examine events with kubectl describe pod.

9. API Server Unreachable:
 - Verify API server status.
 - Check kubeconfig file for correct API server address.

10. Network Policy Restrictions:
 - Inspect network policies.
 - Ensure correct podSelector and policy rules.

11. RBAC Permission Issues:
 - Validate service account permissions.
 - Check ClusterRoleBindings and RoleBindings.

12. PersistentVolumeClaims (PVC) Pending:
 - Check available storage classes.
 - Ensure the requested storage is available.

13. NodePort Service Not Accessible:
 - Confirm the node's firewall allows the specified port.
 - Check Service and NodePort configurations.

14. Kube-proxy Issues:
 - Inspect kube-proxy logs for errors.
 - Verify kube-proxy configuration.

15. DNS Resolution Problems:
 - Verify CoreDNS status.
 - Check DNS policy and cluster DNS settings.

16. Pod Eviction Due to Resource Pressure:
 - Check resource usage across nodes.
 - Adjust resource limits or add more nodes.

17. NodeOutOfMemory:
 - Inspect node memory usage.
 - Check for memory leaks in applications.

18. Volume Mount Permissions:
 - Ensure correct file permissions in mounted volumes.
 - Verify SELinux or AppArmor settings.

19. Ingress Controller Misconfiguration:
 - Validate Ingress resource configurations.
 - Check the Ingress controller logs.

20. ETCD Cluster Issues:
 - Check etcd pod logs.
 - Verify etcd cluster health.

21. CNI Plugin Issues:
 - Verify CNI plugin status on nodes.
 - Check plugin configurations and logs.

22. Invalid Labels or Selectors:
 - Ensure correct labels and selectors in resources.
 - Use kubectl get with appropriate labels.

-------------------------------------------------------------------

### **1. Application Pods Are Stuck in CrashLoopBackOff**

**Scenario:** Application pods repeatedly restart due to misconfigurations or runtime errors.

**Commands and Steps:**

1. Check pod logs for specific errors:
    
    ```bash
    
    kubectl logs <pod-name>
    
    ```
    
2. Inspect pod events and resource configurations:
    
    ```bash
    
    kubectl describe pod <pod-name>
    
    ```
    
3. Debug application configurations in the YAML:
    
    ```bash
    
    kubectl get pod <pod-name> -o yaml
    
    ```
    
4. Fix the configuration and redeploy:
    
    ```bash
    
    kubectl apply -f <deployment.yaml>
    
    ```
    

**Explanation:**

Logs may indicate issues like missing environment variables, incorrect database credentials, or runtime exceptions. In one scenario, fixing an incorrect database URL stored in a Secret resolved the error.

---

### **2. Node Not Ready**

**Scenario:** A cluster node is in a `NotReady` state, disrupting workloads.

**Commands and Steps:**

1. Describe the node to identify underlying issues:
    
    ```bash
    
    kubectl describe node <node-name>
    
    ```
    
2. Check kubelet logs for errors:
    
    ```bash
    
    journalctl -u kubelet
    
    ```
    
3. Verify disk and memory usage:
    
    ```bash
    
    kubectl top node <node-name>
    
    ```
    
4. Drain the node and restart services:
    
    ```bash
    
    kubectl drain <node-name> --ignore-daemonsets --force
    sudo systemctl restart kubelet
    
    ```
    

**Explanation:**

The issue was resolved by cleaning up disk space caused by dangling Docker images and restarting kubelet.

---

### **3. Persistent Volume Stuck in Pending State**

**Scenario:** Persistent Volume Claim (PVC) fails to bind to a Persistent Volume (PV).

**Commands and Steps:**

1. Describe the PVC to check for errors:
    
    ```bash
    
    kubectl describe pvc <pvc-name>
    
    ```
    
2. Verify available PVs:
    
    ```bash
    kubectl get pv
    
    ```
    
3. Check if StorageClass matches:
    
    ```bash
    
    kubectl get storageclass
    
    ```
    
4. Update the PVC to use the correct StorageClass:
    
    ```yaml
    
    storageClassName: <correct-storage-class>
    
    ```
    

**Explanation:**

The PVC had an incorrect `StorageClassName`. Fixing it allowed the provisioner to allocate storage.

---

### **4. Service Unable to Route Traffic to Pods**

**Scenario:** A Kubernetes Service is unable to forward traffic to its backend pods.

**Commands and Steps:**

1. Verify if endpoints exist for the service:
    
    ```bash
    
    kubectl get endpoints <service-name>
    
    ```
    
2. Ensure pod labels match the service selector:
    
    ```bash
    
    kubectl get pods --show-labels
    
    ```
    
3. Debug internal connectivity:
    
    ```bash
    
    kubectl exec -it <pod-name> -- curl <service-name>:<port>
    
    ```
    

**Explanation:**

A mismatch between pod labels and the service selector caused the issue. Fixing the service YAML resolved it.

---

### **5. High Pod Evictions on Cluster**

**Scenario:** Pods are being frequently evicted due to resource pressure on nodes.

**Commands and Steps:**

1. Analyze node resource usage:
    
    ```bash
    
    kubectl top nodes
    kubectl describe node <node-name>
    
    ```
    
2. Adjust pod priorities to protect critical workloads:
    
    ```yaml
    
    priorityClassName: high-priority
    
    ```
    
3. Scale up the cluster:
    
    ```bash
    
    kubectl scale nodes <node-group> --replicas=<desired-count>
    
    ```
    

**Explanation:**

Evictions were caused by memory pressure. Adding additional nodes to the cluster and using pod priorities mitigated the issue.

---

### **6. Liveness and Readiness Probes Failing**

**Scenario:** Pods fail health checks, resulting in restarts or unavailability.

**Commands and Steps:**

1. Debug probe endpoints:
    
    ```bash
    
    kubectl exec -it <pod-name> -- curl localhost:<probe-port>/<probe-path>
    
    ```
    
2. Adjust probe configuration:
    
    ```yaml
    
    livenessProbe:
      httpGet:
        path: /healthz
        port: 8080
      initialDelaySeconds: 20
    
    ```
    
3. Redeploy pods:
    
    ```bash
    
    kubectl rollout restart deployment <deployment-name>
    
    ```
    

**Explanation:**

The application startup was slow, causing probe failures. Increasing the `initialDelaySeconds` resolved the issue.

---

### **7. HPA Not Scaling Pods**

**Scenario:** The Horizontal Pod Autoscaler (HPA) does not scale workloads as expected.

**Commands and Steps:**

1. Check HPA configuration and metrics:
    
    ```bash
    
    kubectl describe hpa <hpa-name>
    
    ```
    
2. Verify pod resource requests and limits:
    
    ```yaml
    
    resources:
      requests:
        cpu: "250m"
        memory: "256Mi"
      limits:
        cpu: "500m"
        memory: "512Mi"
    
    ```
    
3. Check metrics availability:
    
    ```bash
    
    kubectl top pod
    
    ```
    
4. Restart metrics server if needed:
    
    ```bash
    
    kubectl rollout restart deployment metrics-server -n kube-system
    
    ```
    

**Explanation:**

HPA was not scaling due to missing resource limits in the deployment. Adding appropriate CPU/memory limits enabled scaling.

---

### **8. ConfigMap Changes Not Applied**

**Scenario:** Updates to ConfigMaps do not reflect in running pods.

**Commands and Steps:**

1. Verify the updated ConfigMap:
    
    ```bash
    
    kubectl get configmap <configmap-name> -o yaml
    
    ```
    
2. Restart pods to load the new configuration:
    
    ```bash
    
    kubectl rollout restart deployment <deployment-name>
    
    ```
    

**Explanation:**

ConfigMaps are not automatically updated in pods. Restarting the pods applied the changes.

---

### **9. DNS Issues in Kubernetes Cluster**

**Scenario:** Pods are unable to resolve service names.

**Commands and Steps:**

1. Check CoreDNS pods:
    
    ```bash
    
    kubectl get pods -n kube-system -l k8s-app=kube-dns
    
    ```
    
2. Validate DNS functionality inside a pod:
    
    ```bash
    
    kubectl exec -it <pod-name> -- nslookup <service-name>
    
    ```
    
3. Inspect CoreDNS ConfigMap:
    
    ```bash
    
    kubectl describe configmap coredns -n kube-system
    
    ```
    

**Explanation:**

A misconfigured DNS entry in CoreDNS was corrected, restoring name resolution.

---

### **10. Pods Stuck in ContainerCreating State**

**Scenario:** Pods remain in the `ContainerCreating` state and fail to start.

**Commands and Steps:**

1. Check pod events:
    
    ```bash
    
    kubectl describe pod <pod-name>
    
    ```
    
2. Validate image pull access:
    
    ```bash
    
    kubectl logs <pod-name> -c <container-name>
    
    ```
    
3. Inspect node resources:
    
    ```bash
    
    kubectl top node <node-name>
    
    ```
    
4. Recreate pods if necessary:
    
    ```bash
    
    kubectl delete pod <pod-name> && kubectl apply -f <pod-config.yaml>
    
    ```
    

**Explanation:**

Image pull failures caused by missing credentials were fixed by adding a proper `imagePullSecrets` configuration.

---

---

### **11. Kubernetes Cluster Upgrade Issues**

**Scenario:** A Kubernetes cluster upgrade causes API server downtime or breaks certain workloads.

**Commands and Steps:**

1. Backup etcd data:
    
    ```bash
    
    ETCDCTL_API=3 etcdctl snapshot save snapshot.db --endpoints=<etcd-endpoint>
    
    ```
    
2. Upgrade `kubeadm`:
    
    ```bash
    sudo apt-get update && sudo apt-get install -y kubeadm
    
    ```
    
3. Apply the upgrade:
    
    ```bash
    
    sudo kubeadm upgrade apply <target-version>
    
    ```
    
4. Upgrade `kubelet` and `kubectl`:
    
    ```bash
    
    sudo apt-get install -y kubelet kubectl
    sudo systemctl restart kubelet
    
    ```
    
5. Drain and upgrade nodes:
    
    ```bash
    
    kubectl drain <node-name> --ignore-daemonsets --force
    sudo kubeadm upgrade node
    
    ```
    
6. Uncordon nodes:
    
    ```bash
    
    kubectl uncordon <node-name>
    
    ```
    

**Explanation:**

Incompatibilities with older CRDs or Helm charts caused issues during an upgrade. Backing up etcd and upgrading CRDs before cluster upgrade prevented outages.

---

### **12. Overloaded API Server**

**Scenario:** High API server load affects performance and causes timeouts.

**Commands and Steps:**

1. Check API server logs for excessive requests:
    
    ```bash
    
    kubectl logs -n kube-system <api-server-pod-name>
    
    ```
    
2. Identify frequent API users:
    
    ```bash
    
    kubectl top pod -n kube-system
    
    ```
    
3. Enable client-side caching in kube-proxy:
    
    ```yaml
    
    kind: ConfigMap
    apiVersion: v1
    metadata:
      name: kube-proxy
      namespace: kube-system
    data:
      config.conf: |
        clusterCIDR: <CIDR-range>
        mode: iptables
    
    ```
    
4. Add replicas for HA:
    
    ```bash
    
    kubectl scale deployment kube-apiserver --replicas=3 -n kube-system
    
    ```
    

**Explanation:**

Excessive requests from a CI/CD pipeline were rate-limited, and scaling API server replicas ensured availability.

---

### **13. Ingress Not Working**

**Scenario:** An Ingress resource fails to route external traffic to backend services.

**Commands and Steps:**

1. Describe the Ingress resource:
    
    ```bash
    
    kubectl describe ingress <ingress-name>
    
    ```
    
2. Verify backend services:
    
    ```bash
    
    kubectl get svc <service-name>
    
    ```
    
3. Check ingress controller logs:
    
    ```bash
    
    kubectl logs <ingress-controller-pod-name>
    
    ```
    
4. Test DNS resolution:
    
    ```bash
    
    nslookup <ingress-host>
    
    ```
    

**Explanation:**

A misconfigured backend service port caused the failure. Correcting the service configuration resolved the issue.

---

### **14. Pod Scheduling Fails Despite Node Availability**

**Scenario:** Pods are not scheduled on available nodes due to constraints like taints, tolerations, or affinity rules.

**Commands and Steps:**

1. Check pod events:
    
    ```bash
    
    kubectl describe pod <pod-name>
    
    ```
    
2. List taints on nodes:
    
    ```bash
    
    kubectl describe node <node-name>
    
    ```
    
3. Add tolerations to pod spec:
    
    ```yaml
    
    tolerations:
      - key: "node.kubernetes.io/memory-pressure"
        operator: "Exists"
        effect: "NoSchedule"
    
    ```
    
4. Review affinity rules:
    
    ```yaml
    
    affinity:
      nodeAffinity:
        requiredDuringSchedulingIgnoredDuringExecution:
          nodeSelectorTerms:
            - matchExpressions:
                - key: "disktype"
                  operator: In
                  values:
                    - ssd
    
    ```
    

**Explanation:**

A misconfigured `nodeSelector` prevented scheduling. Updating affinity rules resolved the issue.

---

### **15. StatefulSet Pods Failing After Restart**

**Scenario:** After restarting StatefulSet pods, data or application state is missing.

**Commands and Steps:**

1. Verify Persistent Volume Claims (PVCs):
    
    ```bash
    
    kubectl get pvc -l app=<statefulset-name>
    
    ```
    
2. Check volume mounts in pods:
    
    ```bash
    
    kubectl describe pod <pod-name>
    
    ```
    
3. Validate headless service:
    
    ```bash
    
    kubectl get svc <headless-service-name>
    
    ```
    
4. Fix missing volumes and redeploy:
    
    ```yaml
    
    volumeClaimTemplates:
      - metadata:
          name: data
        spec:
          accessModes: ["ReadWriteOnce"]
          resources:
            requests:
              storage: 10Gi
    
    ```
    

**Explanation:**

The issue occurred because of missing PVCs. Ensuring PVCs persisted across restarts resolved the problem.

---

### **16. Kubernetes Network Policies Blocking Traffic**

**Scenario:** Network policies unintentionally block traffic between pods.

**Commands and Steps:**

1. Inspect network policy:
    
    ```bash
    
    kubectl describe networkpolicy <policy-name>
    
    ```
    
2. Validate pod labels:
    
    ```bash
    
    kubectl get pods --show-labels
    
    ```
    
3. Test connectivity:
    
    ```bash
    
    kubectl exec -it <pod-name> -- curl <target-pod-ip>
    
    ```
    
4. Update network policy:
    
    ```yaml
    
    ingress:
      - from:
          - podSelector:
              matchLabels:
                role: frontend
    
    ```
    

**Explanation:**

The policy incorrectly restricted traffic to backend pods. Adjusting the `podSelector` fixed the issue.

---

### **17. Pods Stuck in Terminating State**

**Scenario:** Pods remain in a `Terminating` state even after multiple deletion attempts.

**Commands and Steps:**

1. Force-delete the pod:
    
    ```bash
    
    kubectl delete pod <pod-name> --grace-period=0 --force
    
    ```
    
2. Check if finalizers block deletion:
    
    ```bash
    
    kubectl get pod <pod-name> -o json | jq '.metadata.finalizers'
    
    ```
    
3. Remove finalizers:
    
    ```bash
    
    kubectl patch pod <pod-name> -p '{"metadata":{"finalizers":[]}}' --type=merge
    
    ```
    

**Explanation:**

The issue was caused by stuck finalizers in the pod metadata. Removing the finalizers allowed the pod to terminate.

---

### **18. Cluster Autoscaler Not Scaling Nodes**

**Scenario:** Cluster Autoscaler fails to add nodes during high workload demand.

**Commands and Steps:**

1. Inspect Autoscaler logs:
    
    ```bash
    
    kubectl logs -n kube-system deployment/cluster-autoscaler
    
    ```
    
2. Verify resource requests:
    
    ```yaml
    
    resources:
      requests:
        cpu: "500m"
        memory: "512Mi"
    
    ```
    
3. Check scaling limits:
    
    ```bash
    
    kubectl describe configmap cluster-autoscaler-config -n kube-system
    
    ```
    
4. Adjust node group size:
    
    ```bash
    
    aws autoscaling set-desired-capacity --auto-scaling-group-name <asg-name> --desired-capacity <new-size>
    
    ```
    

**Explanation:**

A misconfigured maximum node count prevented scaling. Updating the configuration resolved the issue.

---

### **19. Metrics Server Failing in Custom Cluster**

**Scenario:** Metrics Server fails to provide pod and node metrics in a self-managed cluster.

**Commands and Steps:**

1. Inspect Metrics Server logs:
    
    ```bash
    
    kubectl logs -n kube-system deployment/metrics-server
    
    ```
    
2. Verify `kubelet` arguments:
    
    ```bash
    
    ps aux | grep kubelet
    
    ```
    
3. Update Metrics Server deployment:
    
    ```yaml
    
    spec:
      containers:
        - name: metrics-server
          args:
            - --kubelet-insecure-tls
    
    ```
    

**Explanation:**

The kubelet certificates were not properly configured. Adding the `--kubelet-insecure-tls` flag resolved the issue.

---

### **20. Pods Running on Unintended Nodes**

**Scenario:** Pods are scheduled on nodes that do not meet application requirements.

**Commands and Steps:**

1. Verify node labels:
    
    ```bash
    
    kubectl get nodes --show-labels
    
    ```
    
2. Update pod affinity rules:
    
    ```yaml
    
    nodeSelector:
      disktype: ssd
    
    ```
    
3. Drain unintended nodes:
    
    ```bash
    
    kubectl drain <node-name> --ignore-daemonsets --force
    
    ```
    
4. Reschedule pods:
    
    ```bash
    
    kubectl rollout restart deployment <deployment-name>
    
    ```
    

**Explanation:**

A missing `nodeSelector` caused pods to be scheduled incorrectly. Adding the correct label fixed the issue.
