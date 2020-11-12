############
Installation
############

***************
Installing HeLx
***************

Standard K8S Install
==========================

Prerequisites 
-------------

These instructions assume you have cluster admin permissions for your cluster. 

1. Set up a loadbalancer (we use MetalLB) for the cluster
2. Set up an NFS server for persistent storage 
3. Create a namespace 
   kubectl create namespace <<helx-username>>
4. Create two NFS subdirectories based on namespace
   /srv/k8s-pvs/namespace/appstore
   /srv/k8s-pvs/namespace/stdnfs
5. Allocate an IP address for your helx website i.e. 192.168.0.1
6. Create a DNS record for your helx website i.e. helx.example.com
7. Create a TLS certificate for your website and a secret for the certificate

PV and PVC for appstore and stdnfs will be created as part of the deployment script later. 

Install helm3
-------------

1. Download_ any helm3 release.
2. Unpack it using tar (tar -zxvf helm-v3.0.0-linux-amd64.tar.gz).
3. Move the helm binary to a desired location (mv linux-amd64/helm
   /usr/local/bin/helm).
   
.. _Download: https://github.com/helm/helm/releases 

Get a GitHub or Google Oauth client id and token
------------------------------------------------

GitHub
^^^^^^

1. In your GitHub account, go to Settings->Developer Settings
2. On the left panel, select OAuth Apps -> New OAuth App
3. Enter the application name i.e helx-github
4. Set Homepage URL -> https://[your hostname]/accounts/login 
5. Set Authorization Callback URL -> https://[your hostname]/accounts/github/login/callback/
6. Record the values for GITHUB_NAME, GITHUB_CLIENT_ID, and GITHUB_SECRET to be used in deployment later

Google
^^^^^^
1. Log in to your GCP account and navigate to API & Services->Credentials
2. Create a new OAuth client ID with the application type of Web application
3. Set Authorized JavaScript origins URIs -> https://[your hostname] 
4. Set Authorized redirect URIs -> to https://[your hostname]/accounts/google/login/callback/
5. After the credentials are created record GOOGLE_NAME, GOOGLE_CLIENT_ID, and GOOGLE_SECRET to be used in deployment later 

Access to install script
------------------------
1. Use Git to clone the devops repo using the following command: 
git clone https://github.com/helxplatform/devops.git
2. Navigate to devops/bin and copy env-vars-template.sh to an env specific properties file for your cluster 
cp env-vars-template.sh env-vars-clustername-helx.sh
3. Edit the env vars file to be more specific to the cluster env you have set up earlier. 
4. Add variables for GITHUB_NAME, GITHUB_CLIENT_ID, and GITHUB_SECRET in the variables file and assign the corresponding values after the OAuth App is created.

Deploy
------
To deploy tycho, ambassador, nginx, and appstore use "deploy all"
./k8s-apps.sh -c env-vars-blackbalsam-igilani-helx.sh deploy all

To deploy specific components such as tycho use 
./k8s-apps.sh -c env-vars-blackbalsam-igilani-helx.sh deploy tycho

To delete all deployments 
./k8s-apps.sh -c env-vars-blackbalsam-igilani-helx.sh delete apps

Please note that PVs/PVCs will need to be deleted separately. To delete everything including the PVs and PVCs, you can use
./k8s-apps.sh -c env-vars-blackbalsam-igilani-helx.sh delete all

To delete a specific deployment
./k8s-apps.sh -c env-vars-blackbalsam-igilani-helx.sh tycho

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
