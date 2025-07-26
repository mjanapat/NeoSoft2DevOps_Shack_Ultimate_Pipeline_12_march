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
-------
To access sonarqube, Jenkins and Nexus from my loacal machine add another adapter 
🔧 Next Steps for a Clean Setup
- Add Adapter3 (Bridged)
- Choose your active physical adapter (e.g., Ethernet or Wi-Fi).
- Enable Promiscuous Mode: Allow All if required for traffic sniffing or agent communication.
- Inside VM: Confirm Bridged IP
- Run ip a or ifconfig inside your Jenkins/Sonar/Nexus VM.
- Note the IP address under Adapter3 — it should match your LAN/subnet (e.g., 192.168.1.x).
- Ping Test
- From the Jenkins VM, ping the cluster nodes using their Host-Only IPs and Bridged IPs.
- Ensure full reachability — this validates the dual-net handshake.
- Optional Routing Check
ip route


- Confirm your traffic doesn’t unintentionally bypass the cluster subnet.

🧭 Final Adapter View (Target Setup)
| Adapter | Type | Purpose | 
| Adapter1 | Host-Only | Kubernetes control plane access | 
| Adapter2 | NAT | Internet for tool downloads | 
| Adapter3 | Bridged | CI/CD ↔ Cluster communication | 



If you'd like, I can sketch out your final network topology with IP ranges and data flow for Jenkins ↔ Cluster. Or we can codify all this into a reusable recovery playbook. Just say the word.





