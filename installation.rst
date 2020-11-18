#############
Installation
#############

****************
Installing HeLx
****************

Prerequisites
===============
1. Install GCloud sdk https://cloud.google.com/sdk/docs/install and configure for your project 
2. Install Helm3 https://helm.sh/docs/intro/install/
3. Install Git https://git-scm.com/book/en/v2/Getting-Started-Installing-Git

Optional:

4. Set up GitHub or Google OAuth credentials if configuring social auth for your install

GitHub
-------
1. In your GitHub account, go to Settings->Developer Settings
2. On the left panel, select OAuth Apps -> New OAuth App
3. Enter the application name i.e ``helx-github``
4. Set Homepage URL -> ``https://[your hostname]/accounts/login``
5. Set Authorization Callback URL -> ``https://[your hostname]/accounts/github/login/callback/``
6. Record the values for ``GITHUB_NAME``, ``GITHUB_CLIENT_ID``, and ``GITHUB_SECRET`` to be used in deployment later

Google
-------
1. Log in to your GCP account and navigate to API & Services->Credentials
2. Create a new OAuth client ID with the application type of Web application
3. Set Authorized JavaScript origins URIs -> ``https://[your hostname]`` 
4. Set Authorized redirect URIs -> to ``https://[your hostname]/accounts/google/login/callback/``
5. After the credentials are created record ``GOOGLE_NAME``, ``GOOGLE_CLIENT_ID``, and ``GOOGLE_SECRET`` to be used in deployment later 

GKE Install using Helm
======================

Access to a GKE Cluster
-----------------------
Obtain access to a GKE cluster (For instructions on setting up a cluster, refer to https://cloud.google.com/kubernetes-engine/docs/quickstart)

Deployment
-----------
1. Clone the devops repo into a directory on your local machine using the following command: 
   git clone https://github.com/helxplatform/devops.git 
   This will clone the repo into "devops" folder in your current working directory.
2. Install HeLx using the following command:
   ``helm install helx ./devops/helx``
   
Following is some output from the helm install command:
   
:: 

   [vagrant@localhost helxplatform]$ helm install helx devops/helx
   NAME: helx
   LAST DEPLOYED: Tue Nov 17 21:40:55 2020
   NAMESPACE: default
   STATUS: deployed
   REVISION: 1
   NOTES:
   Use the following commands to get information about how to log in to the HeLx website.
   DJANGO_ADMIN_USERNAME=`kubectl get secret csappstore-secret -o jsonpath="{.data.APPSTORE_DJANGO_USERNAME}" | base64 --decode`
   DJANGO_ADMIN_PASSWORD=`kubectl get secret csappstore-secret -o jsonpath="{.data.APPSTORE_DJANGO_PASSWORD}" | base64 --decode`
   HELX_IP=`kubectl get svc nginx-revproxy -o jsonpath="{.status.loadBalancer.ingress[*].ip}"`
   printf "Django admin username: $DJANGO_ADMIN_USERNAME\nDjango admin password: $DJANGO_ADMIN_PASSWORD\nHeLx URL: http://$HELX_IP\nDjango admin URL: http://$HELX_IP/admin\n"
 
   Example:
   [vagrant@localhost helxplatform]$ DJANGO_ADMIN_USERNAME=`kubectl get secret csappstore-secret -o jsonpath="{.data.APPSTORE_DJANGO_USERNAME}" | base64 --decode`
   [vagrant@localhost helxplatform]$ DJANGO_ADMIN_PASSWORD=`kubectl get secret csappstore-secret -o jsonpath="{.data.APPSTORE_DJANGO_PASSWORD}" | base64 --decode`
   [vagrant@localhost helxplatform]$ HELX_IP=`kubectl get svc nginx-revproxy -o jsonpath="{.status.loadBalancer.ingress[*].ip}"`
   [vagrant@localhost helxplatform]$ printf "Django admin username: $DJANGO_ADMIN_USERNAME\nDjango admin password: $DJANGO_ADMIN_PASSWORD\nHeLx URL: http://$HELX_IP\nDjango admin URL: http://$HELX_IP/admin\n"
   Django admin username: admin
   Django admin password: sHoc6OqUNIhRHs1YHhtF
   HeLx URL: http://34.73.96.240
   Django admin URL: http://34.73.96.240/admin
 
Once logged in as the django admin you can go to the HeLx URL and navigate to the apps section to create new apps. 
  
To create and launch apps as an admin, navigate to http://34.73.96.240/apps
   
Adding a user and OAuth credentials
------------------------------------
1. Use django admin panel to add a new user for your new HeLx instance.
   admin->Users->Add New User
2. Whitelist the newly created user
   admin->Authorized Users->Add New
3. Set up GitHub/Google OAuth accounts 
4. Add social auth credentials in django admin for new user


Cleanup
-------------
To delete HeLx run this command:

::
   helm delete helx

NOTE: You will need to delete any apps created with HeLx using the web UI or manually with kubectl commands.

Standard K8S Install Using an HeLx install script
=================================================

Prerequisites 
-------------

These instructions assume you have cluster admin permissions for your cluster. 

1. Set up a loadbalancer (we use MetalLB) for the cluster
2. Set up an NFS server for persistent storage 
3. Create a namespace 
   kubectl create namespace <<helx-username>>
4. Create two NFS subdirectories based on namespace
   ``/srv/k8s-pvs/namespace/appstore``
   ``/srv/k8s-pvs/namespace/stdnfs``
5. Allocate an IP address for your helx website i.e. ``192.168.0.1``
6. Create a DNS record for your helx website i.e. helx.example.com
7. Create a TLS certificate for your website and a secret for the certificate

PV and PVC for appstore and stdnfs will be created as part of the deployment script later. 

Access to install script
------------------------
1. Use Git to clone the devops repo using the following command: 
git clone https://github.com/helxplatform/devops.git
2. Navigate to devops/bin and copy env-vars-template.sh to an env specific properties file for your cluster 
cp env-vars-template.sh env-vars-clustername-helx.sh
3. Edit the env vars file to be more specific to the cluster env you have set up earlier. 
4. Add variables for ``GITHUB_NAME``, ``GITHUB_CLIENT_ID``, and ``GITHUB_SECRET`` in the variables file and assign the corresponding values after the OAuth App is created.

Deploy
------
To deploy tycho, ambassador, nginx, and appstore use "deploy all"
``./k8s-apps.sh -c env-vars-blackbalsam-igilani-helx.sh deploy all``

To deploy specific components such as tycho use 
``./k8s-apps.sh -c env-vars-blackbalsam-igilani-helx.sh deploy tycho``

Cleanup 
-------
To delete all deployments 
``./k8s-apps.sh -c env-vars-blackbalsam-igilani-helx.sh delete apps``

Please note that PVs/PVCs will need to be deleted separately. To delete everything including the PVs and PVCs, you can use
``./k8s-apps.sh -c env-vars-blackbalsam-igilani-helx.sh delete all``

To delete a specific deployment
``./k8s-apps.sh -c env-vars-blackbalsam-igilani-helx.sh tycho``

.. Contents
.. ========

.. toctree::
   :maxdepth: 4
   :hidden:
     
   DevOps <install_devops>
   More Specific Installs <install_more>

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
