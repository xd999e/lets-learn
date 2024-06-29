# Index

# Pod

The application running as container in a Pod. It's the smallest deployable unit. A Pod is a Kubernetes resource, just like a Deployment or Service. A Pod contains one or more container. If it's containing more than one container it's treated by Kubernetes as a unit. (they are started, stopped together and also executed one the same node)

# Deployment

The Deployment is a recipe to create more Pods. You define what the Pod should look like and how many instances you need. The Deployment monitors the Pods & automatically creates or deletes a Pod to match the number of instances requested.

# Service

Internal load balancers are called **Services**. ("interal only"). Services can be exposed in different ways by specifying a `type` in the spec of the Service

- `ClusterIP` (default) - Exposes the Service on an internal IP in the cluster. This type makes the Service only reachable from within the cluster.
- `NodePort` - Exposes the Service on the same port of each selected Node in the cluster using NAT. Makes a Service accessible from outside the cluster using `<NodeIP>`:`<NodePort>`. Superset of ClusterIP.
- `LoadBalancer` - Creates an external load balancer in the current cloud (if supported) and assigns a fixed, external IP to the Service. Superset of NodePort.
- `ExternalName` - Maps the Service to the contents of the externalName field (e.g. foo.bar.example.com), by returning a `CNAME` record with its value. No proxying of any kind is set up. This type requires v1.7 or higher of kube-dns, or CoreDNS version 0.0.8 or higher.

Services match a set of Pods using `labels` and `selectors`

![[Pasted image 20240615182051.png]]

# Selector

`spec.selector` field is used to select which Pods the Deployment should look after.

# Labels

Labels can be attached to objects at creation time or later on. They can be modified at any time.

# Ingress

Front-facing load balancers which acts as a router is called **Ingress**. ("external access to resources")

# Namespaces

Namespaces are logical sets of resources. You can group Kubernetes resources in Namespaces. Default Namespace is `default`. You can change the context with `kubectl config set-context --current --namespace=<name of namespace>`

Namespaces are only a logical grouping. There is no built-in security mechanism. Pods can still talk to other Pods even if they are in different Namespaces! Namespaces are not designed to isolate workloads!

# Resource

Kubernetes resource definitions are also called "resource manifests" or "resource configurations"

# Kubelet

Each node has a Kubelet, which is an agent for managing the node and communicating with the Kubernetes control plane.

# etcd

Consistent and highly-available key value store used as Kubernetes' backing store for all cluster data.

# Control Plane

Manages the cluster and the nodes that are used to host the running applications. The `Control Plane` coordinates all activities in your cluster, such as scheduling applications, maintaining applications' desired state, scaling applications, and rolling out new updates.

# YAML

Yet Another Markup Language is a human-readable text-based format for specifying configuration-type information. YAML is a superset of JSON, so any valid JSON file is also a valid YAML file.

# Organizing resources

[doc](https://kubernetes.io/docs/concepts/workloads/management/#organizing-resource-configurations)
