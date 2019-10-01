# Kubernetes Notes

## Terminology

**Cluster:** A group of computers that work with each other.
**Master:** coordinates the cluster.
**Nodes:** workers that run the application. Each node has a Kubelet.
**Kubelet:** an agent for managing the node and communicating with the Kubernetes master. The node should also have tools for handling container operations, such as Docker or rkt
