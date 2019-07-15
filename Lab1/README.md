# Lab1 - Loadbalancer Service

## Pre-requisites

    * Deploy the Guestbook:v1

## 

    get service information,
    ```console
    $ kubectl get service guestbook
    NAME        TYPE           CLUSTER-IP      EXTERNAL-IP    PORT(S)          AGE
    guestbook   LoadBalancer   172.21.80.121   169.46.1.252   3000:30783/TCP   21m
    ```

    Your Guestbook is made available via the LoadBalancer at http://169.46.1.252:30783/

    Or describe
    ```console
    $ kubectl describe service guestbook
    Name:                     guestbook
    Namespace:                default
    Labels:                   app=guestbook
    Annotations:              <none>
    Selector:                 app=guestbook
    Type:                     LoadBalancer
    IP:                       172.21.80.121
    LoadBalancer Ingress:     169.46.1.252
    Port:                     <unset>  3000/TCP
    TargetPort:               http-server/TCP
    NodePort:                 <unset>  30783/TCP
    Endpoints:                172.30.150.27:3000,172.30.37.30:3000,172.30.37.31:3000
    Session Affinity:         None
    External Traffic Policy:  Cluster
    Events:
    Type    Reason                Age    From                Message
    ----    ------                ----   ----                -------
    Normal  EnsuringLoadBalancer  3m26s  service-controller  Ensuring load balancer
    Normal  EnsuredLoadBalancer   3m26s  service-controller  Ensured load balancer
    ```

    See that Kubernetes has added a LoadBalancer Ingress IP address.

