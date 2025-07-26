## Following content help you to understand CNI, that we applied in the 2. K8s-setup.md  Step9: about CNI


We deploy Calico at that stage to enable pod-to-pod networking and enforce network policies in the Kubernetes cluster. Here's why it's essential:
🚦 Purpose of Calico Deployment
- Pod Networking (CNI): Kubernetes doesn’t handle pod networking by itself—it relies on a CNI plugin. Calico provides IP address management and routing between pods across nodes.
- Network Policy Enforcement: Calico supports Kubernetes NetworkPolicies, allowing you to control which pods can communicate with each other—critical for security and multi-tenant setups.
- Scalability & Performance: Calico uses BGP or VXLAN for routing, offering high performance and flexibility in large clusters.
- Observability & Troubleshooting: It integrates with tools for flow logs, metrics, and policy auditing.
📍 Why on the Master Node?
- The command kubectl apply -f ... is executed from the master node because:
- It has access to the Kubernetes API server.
- It typically holds the kubeconfig context needed to apply cluster-wide resources.
🧩 What the Manifest Does
Applying calico.yaml:
- Installs Calico components like calico-node (DaemonSet), calico-kube-controllers, and CRDs.
- Sets up IP pools, routing rules, and default network policies.
- Enables pod networking so that nodes can transition from NotReady to Ready.
---------------




 kubeaudit vs kube-bench
| Feature                         | kubeaudit|                                                      | kube-bench |


| Focus      | Detects security misconfigurations in manifests or live cluster            | Checks compliance with CIS Kubernetes Benchmarks | 
| Scope      | Pod specs, containers, namespaces, RBAC, etc.                              | Node-level config, API server flags, etcd, kubelet | 
| Modes      | Manifest, Local, Cluster                                                   | Cluster-only (runs as pod or job) | 
| Output     | Error, Warn, Info with fix suggestions                                    | Pass/Fail with CIS section references | 
| Language   | Go (by Shopify)                                                         | Go (by Aqua Security) | 
| Use Case   | DevSecOps audits, CI/CD manifest checks                                 | Periodic compliance scans, hardening validation | 


🧠 How They Work Together
- kubeaudit is great for catching risky pod-level configs like runAsRoot, missing readOnlyRootFilesystem, or privileged: true.
- kube-bench dives deeper into cluster setup—verifying kubelet flags, API server TLS settings, etc.
If you’re building a security pipeline, you’d typically run:
- kubeaudit during CI/CD or manifest review
- kube-bench periodically on nodes or during cluster bring-up

