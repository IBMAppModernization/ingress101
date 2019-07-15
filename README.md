# Ingress 101

See: https://kubernetes.io/docs/concepts/services-networking/ingress/

IKS provisions a portable public subnet with a default public Ingress ALB, and a portable private subnet, with a def.

## Load Balancing

1. Service TCP-level Load Balancing
    * The default protocol for services is TCP
    * Built-in the Kubernetes fabric: for ReplicaSets via a Service... Every Service that is created, gets three things:
        * Its own (virtual, fixed) IP address
        * A DNS entry in the Kubernetes cluster DNS, for discovery
        * Load-balancing rules that proxy traffic to the Pods that implement the Service
        When packets are sent to the IP, they are load balanced to pods that impment the service. 

    If you don’t need load-balancing and a single Service IP, you can create what are termed “headless” Services, by explicitly specifying "None" for the cluster IP (.spec.clusterIP). For such Services, a cluster IP is not allocated, kube-proxy does not handle these Services, and there is no load balancing or proxying done.

    Kubernetes ServiceTypes:
    * ClusterIP (default)
    * NodePort, expose on static port. A ClusterIP is attached automatically to the NodePort. Allows to set up your own load balancing.
    * LoadBalancer, using a cloud provider’s load balancer. LoadBalancer routes to NodePort, ClusterIP. Creation of the load balancer happens asynchronously, and information about the provisioned balancer is published in the Service’s `.status.loadBalancer` field.
    * ExternalName

2. HTTP Load Balancing and Routing with Ingress
Ingress API was added to Kubernetes was added for HTTP level path and host based load balancing and routing. Instead of the one-to-one relationship between a Service IP address and a set of Pods, an Ingress can use the content of an HTTP request to route requests to different Services.

Nothing happens when Ingress objects are created. You need to also run an Ingress Controller. One of the most popular Ingress Controllers is nginx.

## Annotations

Ingress frequently uses annotations to configure some options depending on the Ingress controller, an example of which is the rewrite-target annotation. 

## Ingress on IBM Cloud
See: https://cloud.ibm.com/docs/containers?topic=containers-ingress-about

Ingress consists of three components: 
* Ingress resources, 
* application load balancers (ALBs), and 
* the multizone load balancer (MZLB).

One Ingress resource is required per namespace with apps to expose.

All clusters created after 24 May 2018 are assigned a subdomain in the new format <cluster_name>.<region_or_zone>.containers.appdomain.cloud. E.g. https://c5.dal12.containers.cloud.ibm.com:23039

### Application load balancer (ALB)
The application load balancer (ALB) is an external load balancer that listens for incoming HTTP, HTTPS, or TCP service requests. The ALB then forwards requests to the appropriate app pod according to the rules defined in the Ingress resource.

When you create a standard cluster, IBM Cloud Kubernetes Service automatically creates a highly available ALB for your cluster and assigns a unique public route to it. The public route is linked to a portable public IP address that is provisioned into your IBM Cloud infrastructure (SoftLayer) account during cluster creation. A default private ALB is also automatically created, but is not automatically enabled.

When you create a standard cluster, IBM Cloud Kubernetes Service automatically provisions a portable public subnet and a portable private subnet. By default, the cluster automatically uses:

* 1 portable public IP address from the portable public subnet for the default public Ingress ALB.
* 1 portable private IP address from the portable private subnet for the default private Ingress ALB.

    ```console
    $ ibmcloud ks albs --cluster <cluster_name_or_id>
    ```

### Multizone Load Balancer (MZLB)
TBD




# Lab

For a Networking 101 with the Guestbook application, see: https://github.com/IBM/kubernetes-networking

