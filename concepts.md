## Concepts Terminology / Namespace Hierarchy

**Cluster:** A group of computers that work with each other.

**Master:** coordinates the cluster.

**Nodes:** workers that run the application. Each node has a Kubelet.

**Kubelet:** an agent for managing the node and communicating with the Kubernetes master. The node should also have tools for handling container operations, such as Docker or rkt.

**Pod:** A group of one or more Containers, tied together for the purposes of administration and networking. It is an abstraction that represents a group of one or more application containers (such as Docker or rkt), and some shared resources for those containers. Those resources include:

-   Shared storage, as Volumes
-   Networking, as a unique cluster IP address
-   Information about how to run each container, such as the container image version or specific ports to use

A Pod models an application-specific "logical host".
A Node can have multiple pods.

**The containers in a Pod share an IP Address and port space, are always co-located and co-scheduled, and run in a shared context on the same Node.**

![hierarchy of a node](./resources/node-overview.svg "hierarchy of a node")

**Service:**  An abstraction which defines a logical set of Pods and a policy by which to access them. Services enable a loose coupling between dependent Pods. A Service is defined using YAML (preferred) or JSON, like all Kubernetes objects. The set of Pods targeted by a Service is usually determined by a `LabelSelector` (see below for why you might want a Service without including selector in the spec).

![services](./resources/services.svg "services")

For more info see [services](./services.md)

A Service routes traffic across a set of Pods. Services are the abstraction that allow pods to die and replicate in Kubernetes without impacting your application. Discovery and routing among dependent Pods (such as the frontend and backend components in an application) is handled by Kubernetes Services.

Services match a set of Pods using [labels and selectors](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/), a grouping primitive that allows logical operation on objects in Kubernetes. Labels are key/value pairs attached to objects and can be used in any number of ways:

Although each Pod has a unique IP address, those IPs are not exposed outside the cluster without a Service. Services allow your applications to receive traffic. Services can be exposed in different ways by specifying a type in the ServiceSpec:

-   `ClusterIP` (default) - Exposes the Service on an internal IP in the cluster. This type makes the Service only reachable from within the cluster.

-   `NodePort` - Exposes the Service on the same port of each selected Node in the cluster using NAT. Makes a Service accessible from outside the cluster using <NodeIP>:<NodePort>. Superset of ClusterIP.

-   `LoadBalancer` - Creates an external load balancer in the current cloud (if supported) and assigns a fixed, external IP to the Service. Superset of NodePort.

-   `ExternalName` - Exposes the Service using an arbitrary name (specified by externalName in the spec) by returning a CNAME record with the name. No proxy is used. This type requires v1.7 or higher of kube-dns.

### Exposing The App using Services

1. List services with `kubectl get services`
2. `kubectl expose [SERVICE NAME] --type="NodePort" --port [CONTAINER PORT]`
This will map `[CONTAINER PORT]` to an ephemeral port.

Example:

```
$ kubectl expose deployment/kubernetes-bootcamp --type="NodePort" --port 8080
service/kubernetes-bootcamp exposed


$ kubectl get services
NAME                  TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
kubernetes            ClusterIP   10.96.0.1      <none>        443/TCP          3m22s
kubernetes-bootcamp   NodePort    10.106.21.43   <none>        8080:32324/TCP   3m2s
```

### Deleting A Service



## Labels

You can label resources like: `kubectl label pod $POD_NAME app=v1`.
And search/query for resources like `kubectl get pods -l app=v1` or
`kubectl get pods -l 'app in (app1, spp, v1)'`
The `kubectl get [RESOURCE]` command takes a `--show-labels` option.
