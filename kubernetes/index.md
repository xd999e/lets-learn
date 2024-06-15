# Index

# Pod

The application running as container in a Pod. It's the smallest deployable unit. A Pod is a Kubernetes resource, just like a Deployment or Service. A Pod contains one or more container. If it's containing more than one container it's treated by Kubernetes as a unit. (they are started, stopped together and also executed one the same node)

# Deployment

The Deployment is a recipe to create more Pods. You define what the Pod should look like and how many instances you need. The Deployment monitors the Pods & automatically creates or deletes a Pod to match the number of instances requested.

# Selector

`spec.selector` field is used to select which Pods the Deployment should look after.

# Service

Internal load balancers are called **Services**. ("interal only")

# Ingress

Front-facing load balancers which acts as a router is called **Ingress**. ("external access to resources")

# Namespaces

Namespaces are logical sets of resources. You can group Kubernetes resources in Namespaces. Default Namespace is `default`. You can change the context with `kubectl config set-context --current --namespace=<name of namespace>`

Namespaces are only a logical grouping. There is no built-in security mechanism. Pods can still talk to other Pods even if they are in different Namespaces! Namespaces are not designed to isolate workloads!

# Resource

Kubernetes resource definitions are also called "resource manifests" or "resource configurations"

# YAML

Yet Another Markup Language is a human-readable text-based format for specifying configuration-type information. YAML is a superset of JSON, so any valid JSON file is also a valid YAML file.

# Organizing resources

[doc](https://kubernetes.io/docs/concepts/workloads/management/#organizing-resource-configurations)
