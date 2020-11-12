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

1. create namespace
2. create NFS subdirectory based on namespace
3. create namespace specific DNS record
4. allocate IP address
5. create namespace specific TLS certificate secret
6. create pv and pvc for appstore
7. create pv and pvc for stdnfs

Install helm3
-------------

1. Download_ any helm3 release.
2. Unpack it using tar (tar -zxvf helm-v3.0.0-linux-amd64.tar.gz).
3. Move the helm binary to a desired location (mv linux-amd64/helm
   /usr/local/bin/helm).
   
.. _Download: https://github.com/helm/helm/releases 

Get a GitHub or Google Oauth client id and token
------------------------------------------------


Access to install script
------------------------
1. Use Git to clone the devops repo using the following command: 
git clone https://github.com/helxplatform/devops.git
2. Navigate to devops/bin and copy env-vars-template.sh to an env specific properties file for your cluster 
cp env-vars-template.sh env-vars-clustername-helx.sh
3. Edit the env vars file to be more specific to the cluster env you have set up earlier. 

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
