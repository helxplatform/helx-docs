############## 
Authentication
############## 

Authentication is concerned with verifying the identity of a principal
and is distinct from determining what actions the principal is entitled
to take in the system. We use the OpenID Connect (OIDC) protocol to
federate user identities from an OIDC identity provider (IdP) like
Google or GitHub. The OIDC protocol is integrated into the system via
open source connectors for the Django environment. This approach entails
configuring the application within the platform of each IdP to permit
and execute the OIDC handshake.

*************
Authorization
*************

Authentication is provided via an OpenID Connect (OIDC) Identity
Provider (IdP) such as GitHub or Google as well as a SAML2 IdP. A SAML2
IdP can be configured on a per customer basis using django-saml2-auth
module. After a principal is authenticated, AppStore uses a request
filter to determine if it is whitelisted. A django middleware intercepts
the request and checks the authenticated user’s email against an
authorized whitelist to grant user access to apps. Certain data such as
credentials for social authentication accounts must not be packaged with
source code. Therefore this data must be dynamically injected during
image deployment using Kubernetes secrets. AppStore offers a command
line interface (CLI) consisting of uniform commands for providing
default values for secrets for local deployment and a way to insert them
for production deployment. It also provides interfaces for creating
system super users, data migration, starting the application, and
packaging executables, among other tasks.

Secrets
=======

Data that serves, for example, as a credential for an authentication,
must be secret. Since it may not be added to source code control, these
values are private to the deployment organization, and must be
dynamically injected. This is handled by using Kubernetes secrets during
deployment, and trivial keys for developer desktops, all within a
uniform process framework. That framework provides a command line
interface (CLI) for creating system super users, data migration,
starting the application, and packaging executables, among other tasks.

Management CLI
--------------

The appstore management CLI provides uniform commands for using the
environment. It provides default values for secrets during local
development and ways to provide secrets in production.

+------------------------------------+----------------------------------------------------------------+
| Command                            | Description                                                    |
+====================================+================================================================+
| bin/appstore tests {product}       | Run automated unit tests with {product} settings.              |
+------------------------------------+----------------------------------------------------------------+
| bin/appstore run {product}         | Run the appstore using {product} settings.                     |
+------------------------------------+----------------------------------------------------------------+
| bin/appstore createsuperuser       | Create admin user with environment variable provided values.   |
+------------------------------------+----------------------------------------------------------------+
| bin/appstore image build           | Build the docker image.                                        |
+------------------------------------+----------------------------------------------------------------+
| bin/appstore image push            | Push the docker image to the repository.                       |
+------------------------------------+----------------------------------------------------------------+
| bin/appstore image run {product}   | Run automated unit tests with {product} settings.              |
+------------------------------------+----------------------------------------------------------------+
| bin/appstore help                  | Run automated unit tests with {product} settings.              |
+------------------------------------+----------------------------------------------------------------+

Testing
-------

Automated testing uses the Python standard unittest and Django testing
frameworks. Tests should be fast enough to run conveniently, and
maximize coverage. For example, the Django testing framework allows for
testing URL routes, middleware and other use interface elements in
addition to the logic of components.

Packaging
---------

Appstore is packaged as a Docker image. It is a non-root container,
meaning the user is not a superuser. It packages a branch of Tycho
cloned within the appstore hierarchy.

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

App Development
===============

During development, environment variables can be set to control
execution:

+-------------------------------------------+---------------------------------------------------------------------+
| Variable                                  | Description                                                         |
+===========================================+=====================================================================+
| DEV\_PHASE=[stub, local, dev, val, prod   | In stub, does not require a Tycho service.                          |
+-------------------------------------------+---------------------------------------------------------------------+
| ALLOW\_DJANGO\_LOGIN=[TRUE, FALSE]        | When true, presents username and password authentication options.   |
+-------------------------------------------+---------------------------------------------------------------------+
| SECRET\_KEY                               | Key for securing the application.                                   |
+-------------------------------------------+---------------------------------------------------------------------+
| OAUTH\_PROVIDERS                          | Contains all the providers(google, github).                         |
+-------------------------------------------+---------------------------------------------------------------------+
| GOOGLE\_CLIENT\_ID                        | Contains the client\_id of the provider.                            |
+-------------------------------------------+---------------------------------------------------------------------+
| GOOGLE\_SECRET                            | Contains the secret key for provider.                               |
+-------------------------------------------+---------------------------------------------------------------------+
| GOOGLE\_NAME                              | Sets the name for the provider.                                     |
+-------------------------------------------+---------------------------------------------------------------------+
| GOOGLE\_KEY                               | Holds the key value for provider.                                   |
+-------------------------------------------+---------------------------------------------------------------------+
| GOOGLE\_SITES                             | Contains the sites for the provider.                                |
+-------------------------------------------+---------------------------------------------------------------------+
| GITHUB\_CLIENT\_ID                        | Contains the client\_id of the provider.                            |
+-------------------------------------------+---------------------------------------------------------------------+
| GITHUB\_SECRET                            | Contains the secret key of the provider.                            |
+-------------------------------------------+---------------------------------------------------------------------+
| GITHUB\_NAME                              | Sets the name for the provider.                                     |
+-------------------------------------------+---------------------------------------------------------------------+
| GITHUB\_KEY                               | Holds the key value for provider.                                   |
+-------------------------------------------+---------------------------------------------------------------------+
| GITHUB\_SITES                             | Contains the sites for the provider.                                |
+-------------------------------------------+---------------------------------------------------------------------+
| APPSTORE\_DJANGO\_USERNAME                | Holds superuser username credentials.                               |
+-------------------------------------------+---------------------------------------------------------------------+
| APPSTORE\_DJANGO\_PASSWORD                | Holds superuser password credentials.                               |
+-------------------------------------------+---------------------------------------------------------------------+
| TYCHO\_URL                                | Contains the url of the running tycho host.                         |
+-------------------------------------------+---------------------------------------------------------------------+
| OAUTH\_DB\_DIR                            | Contains the path for the database directory.                       |
+-------------------------------------------+---------------------------------------------------------------------+
| OAUTH\_DB\_FILE                           | Contains the path for the database file.                            |
+-------------------------------------------+---------------------------------------------------------------------+
| POSTGRES\_DB                              | Contains the connection of the database.                            |
+-------------------------------------------+---------------------------------------------------------------------+
| POSTGRES\_HOST                            | Contains the database host.                                         |
+-------------------------------------------+---------------------------------------------------------------------+
| DATABASE\_USER                            | Contains the database username credentials.                         |
+-------------------------------------------+---------------------------------------------------------------------+
| DATABASE\_PASSWORD                        | Contains the database password credentials.                         |
+-------------------------------------------+---------------------------------------------------------------------+
| APPSTORE\_DEFAULT\_FROM\_EMAIL            | Default email address for appstore.                                 |
+-------------------------------------------+---------------------------------------------------------------------+
| APPSTORE\_DEFAULT\_SUPPORT\_EMAIL         | Default support email for appstore.                                 |
+-------------------------------------------+---------------------------------------------------------------------+
| ACCOUNT\_DEFAULT\_HTTP\_PROTOCOL          | Allows to switch between http and https protocol.                   |
+-------------------------------------------+---------------------------------------------------------------------+

App Metadata
------------

Making application development easy is key to bringing the widest range
of useful tools to the platform so we prefer metadata to code wherever
possible for creating HeLx Apps. Apps are systems of cooperating
processes. These are expressed using
`Docker <https://www.docker.com/>`__ and `Docker
Compose <https://docs.docker.com/compose/>`__. Appstore uses the
`Tycho <https://helxplatform.github.io/tycho-docs/gen/html/index.html>`__
engine to discover and manage Apps. The `Tycho app
metadata <https://github.com/helxplatform/tycho/blob/metadata/tycho/conf/app-registry.yaml>`__
format specifies the details of each application, contexts to which
applications belong, and inheritance relationships between contexts.

Docker compose syntax is used to express cooperating containers
comprising an application. The specifications are stored in
`GitHub <https://github.com/helxplatform/app-support-prototype/tree/develop/dockstore-yaml-proposals>`__,
each in an application specific subfolder. Along with the docker
compose, a ``.env`` file specifies environment variables for the
application. If a file called icon.png is provided, that is used as the
application's icon.

Development Environment
-----------------------

More information coming soon. The following script outlines the process:

::

    #!/bin/bash

    set -ex

    # start fresh
    rm -rf appstore
    #  get a vritualenv
    if [ ! -d venv ]; then
        python3 -m venv venv
    fi
    source venv/bin/activate
    # clone appstore
    if [ ! -d appstore ]; then
        git clone git@github.com:helxplatform/appstore.git
    fi
    cd appstore
    # use metadata branch and install requirements
    git checkout metadata
    cd appstore
    pip install -r requirements.txt

    # configure helx product => braini
    product=braini
    # configure dev mode to stub (run w/o tycho api)
    export DEV_PHASE=stub
    # create and or migrate the database
    bin/appstore updatedb $product
    # create the superuser (admin/admin by default)
    bin/appstore createsuperuser
    # execute automated tests
    bin/appstore tests $product
    # run the appstore at localhost:8000
    bin/appstore run $product
