######################
More Specific Installs
######################

Setup a Kubernetes Cluster
===========================

**Step-1:** Before setting up a Kubernetes cluster we need to enable the
Kubernetes Engine API.

**Step-2:** You can either using web based terminal provided by
google(Google Cloud shell) or run the required command line interfaces
on your own computers terminal.

**Step-3:** Ask Google cloud to create a "Kubernetes cluster" and a
"default" node pool to get nodes from. Nodes represent the hardware and
node pools will keep track how much of a certain type of hardware is
required.

::

    gcloud container clusters create \
      --machine-type n1-standard-2 \
      --image-type ubuntu \
      --num-nodes 2 \
      --zone us-east4-b \
      --cluster-version latest \

To check if the cluster is initialized,

::

    kubectl get node -o wide

**Step-4**: Give your account permissions to grant access to all the
cluster-scoped resources(like nodes) and namespaced resources (like
pods). RBAC (Role-based access control) should be used to regulate access
to resources to a specific user based on the role.

::

    kubectl create clusterrolebinding cluster-admin-binding \
      --clusterrole=cluster-admin \
      --user=

Create a "cluster-admin" role and a cluster role biding to bind the role
to the user.

Setup NFS server
================

Installing NFS server
---------------------

**Step-1**: The NFS server is backed by a google
persistent disk. Make sure it exists prior to the deployment. If the
persistent dosk does not exist, use the following command to create a
disk on a google kubernetes cluster,

::

    gcloud compute disks create gce-nfs-disk --size 10GB --zone us-central1-a

**Step-2**: Run the following command to deploy NFS server.

::

    kubectl apply -R -f nfs-server
