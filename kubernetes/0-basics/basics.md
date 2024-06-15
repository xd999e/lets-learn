# Basics

- You can declaratively describe what you want, not how to get there.
- You use [kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl) to talk with the cluster.
- All workloads are defined in YAML and are submitted to the k8s cluster via [k8s API](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#deployment-v1-apps).
    - You should not use commands like `kubectl run`, `kubectl expose`
    - If you use them, a YAML definition is automatically created for you.
    - The problem is: You do not have the YAML definition on your box or in you code repository.
    - You can get the automatically created definition with e. g. `kubectl get pod <name-of-pod> --output yaml`.
    - You should also not use imperative commands like `kubectl scale`
    - You should also not edit the Deployment with `kubectl edit deployment <deployment>`

- Kubernetes has a lot of resources like: `Deployment`, `Service`, `Ingress`, `Namespace`, ...
    - You can find more about [resources](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/) in the official docs.
    - You can also use the `kubectl explain resource[.field]` command. Example `kubectl explain pod.spec.containers`
    - All resource types have a schema called `kind`.
        - A single instance of a `kind` is called `resource`.
        - A list of instances of a resource is known as a `collection`.

- Do not expose an application with a `Service`. Services are like internal load balancers and can not route traffic depending on the path. You should always use `Ingress` to externally expose an application for the user. Check the documentation for more about [Ingress Controllers](https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/).

# [[index#Pod|Pod]] to Pod communication

- There is no `Pod` to `Pod` communication. Instead `Service` is acting like a middleman.
# Connecting [[index#Deployment|Deployment]] and [[index#Service|Service]]

- `Service` & `Deployment` are not connected. The Service points directly to the Pods.
- You need to understand how Pods & Services are related.

1. The `Service Selector` should match at least one label of the `Pod`.
2. The `Service` `targetPort` should match the `containerPort` of the container inside the `Pod`.
3. The `Service` `port` can be any number. Multiple Services can use the same `port` because they have different IP adrresses assigned.
4. `deployment.spec.template.metadata.labels` & `service.spec.selector` should match.
5. `deployment.spec.selector.matchLabels` & `deployment.spec.template.metadata.labels` should match, so the `Deployment` can track the Pods.

#### Check labels:
`kubectl get pods --show-labels`

Or if you have Pods belonging to several applications:
`kubectl get pods --selector run=hello --show-labels`

# Connecting [[index#Service|Service]] and [[index#Ingress|Ingress]]

- The `Ingress` has to know how to reach the `Service` to then forward the traffic to the Pods.
- The `Ingress` retrieves the right `Service` by name & port.

1. `servicePort`of the Ingress should match `port`of the Service
2. `serviceName`of the Ingress should match the `name` of the Service.

# Overview

![[relation.draw|1000]]
