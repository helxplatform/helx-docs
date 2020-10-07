###############################
HeLx Documentation
###############################

:Release: |release|
:Date: |today|

HeLx puts the most advanced analytical scientific models at
investigator's finger tips using equally advanced cloud native,
container orchestrated, distributed computing systems. HeLx can be
applied in many domains. Its ability to empower researchers to leverage
advanced analytical tools without installation or other infrastructure
concerns has broad reaching benefits.

Contact `HeLx Help <mailto:catalyst-admin@lists.renci.org>`__ with
questions.

********************
Installing HeLx
********************

HeLx Configuration for a Bare-Metal Kubernetes Cluster
==========================================================
 
Requirements for Cluster
--------------------------
 
Tools Needed
^^^^^^^^^^^^^^
* kubectl
   * Needs to be configured with a kubeconfig to access the cluster.
* Helm version 2
 
* Access to a Kubernetes cluster.  Currently we use Kubernetes version 1.17 for development and testing.  Older and newer versions of Kubernetes should also work.
 
* Admin access to a namespace on a Kubernetes cluster as well as the ability to create Persistent Volumes on the cluster.
 
Copy the example configuration file below to a new file and adjust the variables according to the information below.
 
The applications use NFS for persistent storage so the ability to create NFS shares is needed.  Create two directories on the NFS server and specify the locations in the CAT_NFS_PATH (for user and app data) and APPSTORE_OAUTH_NFS_PATH (for Django database) variables.  The NFS server also needs to be specified in the CAT_NFS_SERVER variable.
 
TLS is used for securing the web traffic.  Create a Kubernetes secret in the namespace that HeLx is going to be deployed in that contains the certificate, key, and CA certificate.  Set the NGINX_TLS_SECRET variable to the name of the secret.
 
Set NGINX_IP to the IP that the web service should assigned to.
 
Set a username and password for the DJANGO user.
 
Set a random 50 character password to the SECRET_KEY for Django.
 
In your GitHub account create an OAuth application for HeLx.  The "Homepage URL" in the GitHub OAuth settings should be set to the root of your HeLx website.  The "Authorization callback URL" should be set to "https://www.example.com/github/account".  Once the OAuth App is created take the Client ID and Client Secret and assign them to the GITHUB_CLINET_ID and GITHUB_SECRET.
 
Set a password for CLOUD_TOP_VNC_PW.
 
Set a username and password for the outgoing email service to use.  Currently this is set to use a Google Email App username and password and Google's SMTP server.
 
An Example Configuration File
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
```
export CLUSTER_NAME="helx-cluster" # GKE cluster name and also used for other object names.
export NAMESPACE="helx"
PV_DIR="/srv/k8s-pvs/$NAMESPACE"
export CAT_NFS_SERVER="nfs.example.com"
export CAT_NFS_PATH="$PV_DIR/stdnfs"
export NGINX_SERVERNAME="helx.example.com"
export NGINX_IP="192.168.0.1"
export NGINX_TLS_SECRET="helx-tls-secret"
export APPSTORE_IMAGE="heliumdatastage/appstore:mastercca-v0.0.18"
export TYCHO_API_IMAGE="heliumdatastage/tycho-api:develop-v0.0.22"
export NGINX_IMAGE="heliumdatastage/nginx:cca-v0.0.5"
export APPSTORE_OAUTH_NFS_PATH="$PV_DIR/appstore-oauth"
export APPSTORE_DJANGO_USERNAME="admin"
export APPSTORE_DJANGO_PASSWORD="< SECRET HERE >"
export SECRET_KEY="< SECRET HERE (50 chars) >"
export OAUTH_PROVIDERS="github"
export GITHUB_NAME="<GitHub OAuth App Name>"
export GITHUB_CLIENT_ID="< SECRET HERE >"
export GITHUB_SECRET="< SECRET HERE >"
export CLOUD_TOP_VNC_PW='< SECRET HERE >'
export EMAIL_HOST_USER="email@example.com"
export EMAIL_HOST_PASSWORD="< SECRET HERE >"
```
 
Commands to Deploy HeLx
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
```
git clone https://github.com/helxplatform/devops.git
cd devops/bin
./k8s-apps.sh -c $YOUR_CONFIGURATION_FILE deploy all
```
 
Command to Delete HeLx
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
`./k8s-apps.sh -c $YOUR_CONFIGURATION_FILE delete all`
 
This will leave the PVs and PVCs behind to also remove them add the following to your configuration file.
 
`export APPSTORE_OAUTH_PD_DELETE_W_APP=true`
 
Remove any persistent data from the NFS server manually.

.. Hide the contents from the front page because they are already in
.. the side bar in the Alabaster sphinx style; requires Alabaster
.. config sidebar_includehidden=True (default)

.. Contents
.. ========

.. toctree::
   :maxdepth: 4
   :hidden:
     
   Tycho <tycho.rst>
   AppStore <appstore.rst>
   Installation <installation.rst>
   Current and Upcoming Releases <releases.rst>

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
