############
Installation
############

***************
Installing HeLx
***************

Install helm3
=============

1. Download_ any helm3 release.
2. Unpack it using tar (tar -zxvf helm-v3.0.0-linux-amd64.tar.gz).
3. Move the helm binary to a desired location (mv linux-amd64/helm
   /usr/local/bin/helm).
   
.. _Download: https://github.com/helm/helm/releases 

Install all charts using a single command
------------------------------------------ 
::

    helm install release-name helx/ -n namespace

Install charts individually
---------------------------

**Ambassador** 

1. Edit the values.yaml ( **Important** : service(ClusterIP or LoadBalancer) and prp(True or False)). 
2. helm install **release-name** ambassador/ -n

**AppsStore** 

1. Edit the values.yaml (**Important**: service(ClusterIP or LoadBalancer, ambassador.flag(True or False) and image). 
2. helm install **release-name** appstore/ -n

*LoadBalancer IP won't be necessary when used with nginx reverse proxy
and ambassador. Mapping for AppsStore is defined in the ambassador*

**nginx** 

1. Edit the values.yaml ( **Important**: resolver(coredns.kube-system.svc or kube-dns.kube- system.svc)). 
2. helm install **release-name** nginx/ -n

*Use kube-dns.kube-system.svc for GKE clusters and
coredns.kube-system.svc for on-prem clusters.*

**tycho-api** 

1. Edit the values.yaml ( **Important**: service(ClusterIP or LoadBalancer) and image). 
2. Copy the role.yaml (for PRP) or serviceaccount.yaml(for SciDas and Braini) from /devops/helx to /devops/helx/charts/tycho-api/templates/.

*- role.yaml - set of permissions binding to a single namespace(service
account) using Role and Rolebinding having access to only that
namespace.* *- serviceaccount.yaml - set of permissions binding to a
single namespace(service account) using ClusterRole(cluster-admin) and
ClusterRoleBinding having access to entire cluster.* *- The current
version of tycho-api on Braini/Scidas needs a LoadBalancer IP, but the
later versions we will not need that.*

1. helm install release-name tycho-api/ -n

.. Hide the contents from the front page because they are already in
.. the side bar in the Alabaster sphinx style; requires Alabaster
.. config sidebar_includehidden=True (default)

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
