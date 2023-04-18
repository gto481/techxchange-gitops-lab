# Brewz Application

Brewz is an e-commerce beverage company whose microservices-based application has been designed to deploy into Kubernetes. Deploying this application into Kubernetes allows them to scale this application locally (in the same cluster) for reliability and capacity, as well as scale globally to multiple locations based on customer demand.

This is a simple diagram depicting the design of the Brewz application and its microservices:

<img src="assets/brewz-architecture.png" alt="Brewz application architecture" width="600"/>

## Deploying the Brewz Application

Now that we have a working platform with essential infrastructure applications installed, you as application developer can deploy the Brewz application.

Since the application developers are *also* committed to GitOps, we'll deploy the Brewz application in a similar way to what we've done so far. The Brewz application code is already in Git, so developers are already comfortable with it. It is reasonable for them to also rely on Git for the application's deployment artifacts, and enjoy the benefit of Argo CD handling deployments for them.

Though Argo CD deployed the infrastructure applications with Helm, the developers have chosen to deploy this application using simple Kubernetes manifests.

NOTE: Though we are using a single repository in this lab, it is a good practice to use different repositories for infrastructure apps, and user applications (such as Brewz). This way, appropriate access to each repo is granted based on area of responsibility.

Let's deploy the Brewz application.

1. TODO: add Brewz installation steps here

## Verify the Deployment

1. Verify the installation was successful by logging into Argo CD. Ensure that an application called `brewz` has been installed, and is in sync and healthy.

1. In your browser, open a new tab and enter <TODO: what is this url> to see the Brewz application is live.

1. In your browser, open a new tab and navigate to the XC console and log in if prompted.

1. Navigate to Multi-Cloud App Connect. Ensure your namespace is selected in the top of the left menu.

1. Click Manage -> Load Balancers -> HTTP Load Balancers

1. Review the Load Balancers that have been created for the infrastructure applications that were installed earlier, as well as for the Brewz application that was just installed.

## Examining the Brewz Application

Now that the application has been deployed, examine the applications's deployment manifests.

1. Locate the manifests/brewz (TODO: verify) folder in your repo.

1. The `app.yaml` file contains the application's `Deployment` and `Service` resources to install the applications pods and expose them.

1. The `mongo-init.yaml` file contains a `ConfigMap` resource containing the JSON data used to seed the MongoDB database.

1. The `xc-ingress` file contains an `Ingress` resource with custom annotations. The XC Ingress Controller monitors the cluster for these resources. When this type of resource is deployed, the XC Ingress controller will use the `Ingress` resource details and annotations to:
    - Create an XC Origin pool that points to the NGINX Ingress Controller's service on port 443 in the specified AppStack site
    - Create an XC HTTP Load Balancer for the host name <TODO: need url>, complete with DNS entry
    - Creates a single XC Load Balancer Route for the path `/` set to the Origin Pool created above
    - Sets the Load Balancer to preserve the host name when routing traffic to the NGINX Ingress Controller Service

    <br>

    Note: There are comments in the file indicating which groups of annotations are associated with each XC object created above.

    How does traffic get to our application?

1. Open the `virtual-server.yaml` file. This file contains a `VirtualServer` custom resource that NGINX Ingress Controller will consume to secure, route and shape our traffic destined for the Brewz application. Thanks to the `VirtualServer`'s `host` field, NGINX Ingress Controller will accept and route traffic associated with this host name from the XC Load Balancer to the appropriate services. We will examine the `VirtualService` and `VirtualServerRoute` resources in detail next in this lab.

1. The remaining manifests are Policy resources to provide rate limiting and JWT authorization services to Brewz microservices. We will also look into these later in the lab. (TODO: verify we have time to)

[Continue to next step...](virtualserver.md)