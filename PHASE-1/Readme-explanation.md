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

