********
Overview
********

============
Architecture
============

HeLx puts the most advanced analytical scientific models at
investigator's finger tips using equally advanced cloud native,
container orchestrated, distributed computing systems.

User Experience
------------------

Users browse tools of interest and launch those tools to explore and
analyze available data. From the user's perspective, HeLx is like an
operating system since it runs useful tools. But there's no
installation, the data's already present in the cloud with the
computation, and analyses can be reproducibly shared with others on the
platform.

Compute
------------------ 

The system's underlying computational engine is Kubernetes. HeLx runs in
a Kubernetes cluster and apps are launched and managed within the same
namespace, or administrative context, it uses. Tycho translates the
docker compose representation of systems with one ore more containers
into a Kubernetes representation, coordinates and tracks related
processes, provides utilization information on running processes, and
manages the coordinated deletion of all components when a running
application is stopped.

HeLx’s cloud native design relies on containerization via Docker for
creating reusable modules. These modules are orchestrated using the
Kubernetes scheduler. Kubernetes, available at all major cloud vendors
and multiple federally funded research platforms including the Texas
Advanced Computing Center (TACC) and Pacific Research Platform (PRP)
provides an important layer of portability and reproducibility for HeLx.

Within the Kubernetes setting, the system requires a single public IP
address. That address routes to a web server acting as a reverse proxy.
It partitions the environment into paths requiring an existing
authentication and paths that do not. The Appstore interface handles
users requests to manage workspaces like Jupyter notebooks and the
Nextflow API. For access to applications users have launched, the
reverse proxy first consults Appstore to acquire an existing
authentication token. If a token exists, the request is forwarded to the
programmable reverse proxy which checks the identity of the
authenticated user against the path for the launched application. If the
user is authorized, the request proceeds.

HeLx's primary user interface is the Appstore, which is a Django based
application whose chief responsibilities are authentication and
authorization and providing a means to launch apps relevant to each
compute platform. Authentication is provided via an OpenID Connect
(OIDC) Identity Provider (IdP) such as GitHub or Google as well as a
SAML2 IdP. A SAML2 IdP can be configured on a per customer basis using
django-saml2-auth module. After a principal is authenticated, AppStore
uses a request filter to determine if it is whitelisted. A django
middleware intercepts the request and checks the authenticated user’s
email against an authorized whitelist to grant user access to apps.

Certain data such as credentials for social authentication accounts must
not be packaged with source code. Therefore this data must be
dynamically injected during image deployment using Kubernetes secrets.
AppStore offers a command line interface (CLI) consisting of uniform
commands for providing default values for secrets for local deployment
and a way to insert them for production deployment. It also provides
interfaces for creating system super users, data migration, starting the
application, and packaging executables, among other tasks.

AppStore is packaged as a Docker image. It is a non-root container,
meaning the user is not a superuser. It packages a branch of Tycho
cloned within the AppStore hierarchy.

Launching an application via the AppStore uses the Tycho module which
manages a metadata based representation of all available workspaces.
Workspaces are defined as docker-compose YAML artifacts. These, in turn,
specify sets of cooperating Docker containers constituting a distributed
system. Tycho translates these app specifications into Kubernetes
constructs to instantiate and run the application. During this process,
Tycho also registers the app instance with the Ambassador programmable
reverse proxy specifying that only the user that instantiated the
application is authorized to access the instance. In addition to
configuring the application’s networking and authorization interfaces,
Tycho also configures the workspace’s access to persistent storage. HeLx
requires a single Kubernetes construct known as a persistent volume (PV)
to designate a storage location. It must be configured as
Read-Write-Many to allow multiple Kubernetes deployments to access it
simultaneously. Given that configuration, Tycho will configure a sub
path on the PV for each user’s home directory and mount that path to
each container it creates that is a component of an app.

Storage
------------------ 

HeLx apps, in the near term, will mount a read only file system
containing reference data and to a writable file system for user data.
Storage can also consist of an NFS-rods volume mounted to each container
for access to data stored in iRODS.

Security
------------------ 

HeLx prefers open standard security protocols where available, and
applies standards based best practices, especially from NIST and FISMA,
in several areas.
