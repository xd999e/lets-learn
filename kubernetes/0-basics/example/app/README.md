# How to

## Pre-Requirements

- Make sure you have installed [Docker](https://docs.docker.com/get-docker/).
- make sure you have installed [minikube](https://minikube.sigs.k8s.io/docs/start/)
  - use docker as a driver, when you start minikube `minikube start --driver=docker`
    - enable Ingress after start: `minikube addons enable ingress`
- make sure you have installed [kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl)

- Clone the repo:

    ``` bash
   git clone https://github.com/xd999e/lets-learn.git
   cd lets-learn/0-basics/example/app
    ```

## Create and push docker image

- Create an account on [docker hub](https://hub.docker.com/)
- Create an Access Token (Account Settings -> Security -> Access Token)

1. Login with `docker login`
    `docker login -u <your-docker-hub-user-id>`

2. Built image, tag it and push it to Docker Hub.

    ``` bash
    # built image
    # execute from 'app' directory, where you find the 'Dockerfile'
    docker built -t myapp .

    # tag the image correctly to be able to push it to Docker Hub
    docker tag myapp:latest <your-docker-hub-user-id>/myapp:1.0.0

    # push it to your repository
    docker push <your-docker-hub-user-id>/myapp:1.0.0
    ```

3. Create the Deployment

- Make sure docker is running & minikube is started

    ``` bash
    # check docker status
    systemctl status docker | grep Active:

        Active: active (running) since Thu 2024-06-13 17:31:28 CEST; 4h 8min ago

    # check minikube status
    minikube status

        minikube
        type: Control Plane
        host: Running
        kubelet: Running
        apiserver: Running
        kubeconfig: Configured
    ```

## Create the Deployment

- Create the Deployment using kubectl

    ``` bash
    # make sure you are in the 'app' directory
    kubectl apply -f deployment/app.yaml

        namespace/example-app created
        deployment.apps/myapp created
        service/myapp created
        ingress.networking.k8s.io/myapp created
    ```

## Check the Deployment

- Check every deployed resource

    ``` bash
    # Deployment
    kubectl get deployment -n example-app

        NAME    READY   UP-TO-DATE   AVAILABLE   AGE
        myapp   2/2     2            2           10s

    # Service -> you will have a different CLUSTER-IP
    kubectl get svc -n example-app

        NAME    TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
        myapp   ClusterIP   10.98.166.88   <none>        8080/TCP   10s

    # Ingress -> you will have a different ADDRESS
    kubectl get ingress -n example-app

        NAME    CLASS   HOSTS   ADDRESS        PORTS   AGE
        myapp   nginx   *       192.168.49.2   80      76s
    ```

## Access the app

- Start a `minikube tunnel` or use `kubectl port-forward` to access the app via browser
- With `kubectl port-forward` you can see 'live access logs' during active port-forward.

    Using minikube tunnel:

  - **Important:** you will see different IP Addresses for `route`. Please use your IP!

    ``` bash
    minikube tunnel

        Status:
                machine: minikube
                pid: 124807
                route: 10.96.0.0/12 -> 192.168.49.2
                minikube: Running
                services: []
            errors:
                minikube: no errors
                router: no errors
                loadbalancer emulator: no errors
    ```

    Browser access should be: `http://192.168.49.2/hello` (use your IP!)
    You can also use curl: `curl 192.168.49.2/hello` (use our IP!)

    Using kubectl port-forward:

    ``` bash
    kubectl port-forward service/ingress-nginx-controller -n ingress-nginx 8080:80

        Forwarding from 127.0.0.1:8080 -> 80
        Forwarding from [::1]:8080 -> 80
    ```

    Browser access should be: `http://127.0.0.1:8080/hello`
    You can also use curl: `curl http://127.0.0.1:8080/hello`

## Clean up

- Clean up all resources and docker images

    ``` bash
    kubectl delete namespace example-app

        namespace "example-app" deleted

    docker rmi myapp

        Untagged: myapp:latest

    # check all docker images
    docker images

        REPOSITORY                    TAG       IMAGE ID       CREATED          SIZE
        injvvvgr6q8zca99/myapp        1.0.0     bd26d9db4cda   35 minutes ago   141MB
        gcr.io/k8s-minikube/kicbase   v0.0.44   5a6e59a9bdc0   5 weeks ago      1.26GB

    # remove all images. Do not remove the minikube image!!!
    # for example rm local myapp image
    docker rmi bd26d9db4cda --force

    # do delete minikube
    minikube delete
    ```
