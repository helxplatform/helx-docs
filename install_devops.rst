######
DevOps
######

To deploy HeLx 1.0 on the Google Kubernetes Engine you will need to have
an account with Google Cloud Platform and configure the `Google Cloud
SDK_ on your local system.

.. _SDK: https://cloud.google.com/sdk/docs/quickstarts
Check your Google Cloud SDK is setup correctly.

::

    glcoud info

Decide which directory you want the code to deploy HeLx to be and
execute the following commands to checkout code from their GitHub
repositories. Some variables will need to be changed. These commands
were done in a BASH shell checked on MacOS and will probably work on
Linux, maybe on Windows if you use Cygwin, the Windows Subsystem for
Linux (WSL), or something similar. Most variables can be set as either
environment variables or within the configuration file. Look towards the
top of "devops/bin/gke-cluster.sh" to see a list of variables.

::

    # Set the Google project ID that you want the cluster to be created in.
    PROJECT="A_GOOGLE_PROJECT_ID"
    # Check the Google console for what cluster versions are available to use.
    # This can found in "Master version" property when you start the creation of
    # a cluster.
    CLUSTER_VERSION="1.15.11-gke.3"
    HELXPLATFORM_HOME=$HOME/src/helxplatform
    CLUSTER_NAME="$USER-cluster"
    # Copy "hydroshare-secret.yaml" to
    #   "$HELXPLATFORM_HOME/hydroshare-secret.yaml" or set
    #   HYDROSHARE_SECRET_SRC_FILE to point to it's location below, which is
    #   currently the default value.
    HYDROSHARE_SECRET_SRC_FILE="$HELXPLATFORM_HOME/hydroshare-secret.yaml"
    # The previous variables can also be exported instead of using a configuration
    # file with GKE_CLUSTER_CONFIG exported below.
    export GKE_CLUSTER_CONFIG=$HELXPLATFORM_HOME/env-vars-$USER-test-dev.sh

    # Create directory to hold the source repositories.
    mkdir -p $HELXPLATFORM_HOME
    echo "export CLUSTER_NAME=$CLUSTER_NAME" > $GKE_CLUSTER_CONFIG
    echo "export CLUSTER_VERSION=$CLUSTER_VERSION" >> $GKE_CLUSTER_CONFIG
    echo "export PROJECT=$PROJECT" >> $GKE_CLUSTER_CONFIG
    echo "export HYDROSHARE_SECRET_SRC_FILE=$HYDROSHARE_SECRET_SRC_FILE" >> $GKE_CLUSTER_CONFIG

    cd $HELXPLATFORM_HOME
    git clone https://github.com/helxplatform/CAT_helm.git
    git clone https://github.com/helxplatform/devops.git

    cd $HELXPLATFORM_HOME/devops/bin
    # To create the cluster using the config file specified as a command line
    # argument run this.
    ./gke-cluster.sh -c $GKE_CLUSTER_CONFIG deploy all

    # ...or with the GKE_CLUSTER_CONFIG variable exported you can just run this.
    # ./gke-cluster.sh deploy all

    # Work with cluster and then terminate it.
    echo "###"
    echo "When you are done with the cluster you can terminate it with"
    echo "these commands."
    echo "export GKE_CLUSTER_CONFIG=$HELXPLATFORM_HOME/env-vars-$USER-test-dev.sh"
    echo "cd $HELXPLATFORM_HOME/devops/bin"
    echo "./gke-cluster.sh delete all"
