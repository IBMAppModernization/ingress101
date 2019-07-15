# Lab2 - IKS provided ALBs  
    

1. Check the Default ALBs
    * Login to the IKS Cluster

        ```console
        $ ibmcloud login -a cloud.ibm.com -r us-south -g default
        $ ibmcloud ks cluster-config --cluster <cluster-name>
        ```

    * Query the ALBs,

        ```
        $ ibmcloud ks albs --cluster <cluster-name> 
        OK
        ALB ID                                            Enabled   Status     Type      ALB IP          Zone    Build                          ALB VLAN ID   
        private-cr751377ec84e9428286b9feda3d61f3f9-alb1   false     disabled   private   -               dal10   ingress:477/ingress-auth:331   1788637   
        public-cr751377ec84e9428286b9feda3d61f3f9-alb1    true      enabled    public    169.46.32.186   dal10   ingress:477/ingress-auth:331   2604353
        ```

        * The private ALB is disabled, the public ALB is enabled.

2. Deploy the Guestbook v1 Application

        ```console
        kubectl create -f redis-master-deployment.yaml
        kubectl create -f redis-master-service.yaml
        kubectl create -f redis-slave-deployment.yaml
        kubectl create -f redis-slave-service.yaml
        kubectl create -f guestbook-deployment.yaml
        kubectl create -f guestbook-service.yaml
        ```

    * Get cluster information

        ```console
        $ ibmcloud ks cluster-get <cluster-name>
        ```

    * Or use grep to filter

        ```console
        $ ibmcloud ks cluster-get <cluster-name> | grep Ingress
        Ingress Subdomain:              <cluster-name>.us-south.containers.appdomain.cloud   
        Ingress Secret:                 <cluster-name>  
        ```

    * Access the Guestbook application via the IKS provided public ALB,

        ```console
        $ open http://<cluster-name>.us-south.containers.appdomain.cloud:31724/
        ```

    * Access the Guestbook application via the LoadBalancer service,

        ```console
        $ kubectl get service guestbook
        NAME        TYPE           CLUSTER-IP       EXTERNAL-IP     PORT(S)          AGE
        guestbook   LoadBalancer   172.21.201.147   169.46.32.187   3000:31724/TCP   4m39s

        $ open http://169.46.32.187:31724
        ```

