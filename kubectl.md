## `kubectl` commands

`kubectl` is a way to get information about the cluster.
I'm guessing it's interacting with the API provided by the master. A common format is `kubectl [ACTION] [RESOURCE]`.

### kubectl cluster-info

    $ kubectl cluster-info
    Kubernetes master is running at https://172.17.0.67:8443
    KubeDNS is running at https://172.17.0.67:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

    To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'

### kubectl get nodes

    $ kubectl get nodes
    NAME       STATUS   ROLES    AGE     VERSION
    minikube   Ready    master   6m54s   v1.15.0
    $

### kubectl get pods

    $ kubectl get pods
    NAME                                   READY   STATUS    RESTARTS   AGE
    kubernetes-bootcamp-5b48cfdcbd-9zp6h   1/1     Running   0          2m25s

### Running Commands in a Running Pod

#### kubectl exec $POD_NAME env

Where `$POD_NAME` is a value you got from `kubectl get pods`.

    $ kubectl exec $POD_NAME env
    PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
    HOSTNAME=kubernetes-bootcamp-5b48cfdcbd-9zp6h
    KUBERNETES_PORT_443_TCP_PORT=443
    KUBERNETES_PORT_443_TCP_ADDR=10.96.0.1
    KUBERNETES_SERVICE_HOST=10.96.0.1
    KUBERNETES_SERVICE_PORT=443
    KUBERNETES_SERVICE_PORT_HTTPS=443
    KUBERNETES_PORT=tcp://10.96.0.1:443
    KUBERNETES_PORT_443_TCP=tcp://10.96.0.1:443
    KUBERNETES_PORT_443_TCP_PROTO=tcp

#### kubectl exec -it $POD_NAME bash

To start a bash session...

    $ kubectl exec -ti $POD_NAME bash
    root@kubernetes-bootcamp-5b48cfdcbd-9zp6h:/# hostname
    kubernetes-bootcamp-5b48cfdcbd-9zp6h
    root@kubernetes-bootcamp-5b48cfdcbd-9zp6h:/# exit
    exit
    $ hostname
    minikube

### kubectl describe pods

    $ kubectl describe pods
    Name:           kubernetes-bootcamp-5b48cfdcbd-9zp6h
    Namespace:      default
    Priority:       0
    Node:           minikube/172.17.0.36
    Start Time:     Tue, 01 Oct 2019 14:15:37 +0000
    Labels:         pod-template-hash=5b48cfdcbd
                    run=kubernetes-bootcamp
    Annotations:    <none>
    Status:         Running
    IP:             172.18.0.2
    Controlled By:  ReplicaSet/kubernetes-bootcamp-5b48cfdcbdContainers:
      kubernetes-bootcamp:    Container ID:   docker://5d4547ffe0f6e55b6e9712e5bdfcf1da1b283ca5e9ba048baedf6f97926c39c4    Image:          gcr.io/google-samples/kubernetes-bootcamp:v1
        Image ID:       docker-pullable://jocatalin/kubernetes-bootcamp@sha256:0d6b8ee63bb57c5f5b6156f446b3bc3b3c143d233037f3a2f00e279c8fcc64af
        Port:           8080/TCP    Host Port:      0/TCP
        State:          Running
          Started:      Tue, 01 Oct 2019 14:15:38 +0000
        Ready:          True
        Restart Count:  0
        Environment:    <none>
        Mounts:
          /var/run/secrets/kubernetes.io/serviceaccount from default-token-c6bj6 (ro)
    Conditions:
      Type              Status
      Initialized       True
      Ready             True
      ContainersReady   True
      PodScheduled      True
    Volumes:
      default-token-c6bj6:
        Type:        Secret (a volume populated by a Secret)
        SecretName:  default-token-c6bj6
        Optional:    false
    QoS Class:       BestEffort
    Node-Selectors:  <none>
    Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                     node.kubernetes.io/unreachable:NoExecute for 300s
    Events:
      Type     Reason            Age    From               Message
      ----     ------            ----   ----               -------
      Warning  FailedScheduling  4m35s  default-scheduler  0/1 nodes are available: 1 node(s) had taints that the pod didn't tolerate.
      Normal   Scheduled         4m34s  default-scheduler  Successfully assigned default/kubernetes-bootcamp-5b48cfdcbd-9zp6h to minikube
      Normal   Pulled            4m33s  kubelet, minikube  Container image "gcr.io/google-samples/kubernetes-bootcamp:v1" already present on machine
      Normal   Created           4m33s  kubelet, minikube  Created container kubernetes-bootcamp
      Normal   Started           4m33s  kubelet, minikube  Started container kubernetes-bootcamp
