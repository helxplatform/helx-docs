############
Nextflow API
############


Begin by starting the App as described in the section Creating an
Application_. Select the Nextflow API application.

.. _Application: https://helx-10.readthedocs.io/en/latest/app_create.html?highlight=create%20an%20application

************
Introduction
************

Nextflow enables scalable and reproducible scientific workflows using
software containers. It allows the adaptation of pipelines written in
the most common scripting languages.

Its fluent DSL simplifies the implementation and the deployment of
complex parallel and reactive workflows on clouds and clusters.

Working with Nextflow on HeLx
==============================

**Step-1:** 

-  After selecting the Nextflow API application, you will be taken to the Nextflow API home page, where we can view the launched workflows and create new workflows. 

.. image:: images/nextflow-2.png
    :align: center
    :alt: launch a nextflow API app
    

**Step-2:** 

-  Below is a demo of how to launch a systemsgenetics/kinc workflow. 
-  Click on "Create Workflow" button and fill in the form to give it a "Name" and specify the Pipeline (in this case `systemsgenetics/kinc-nf`). 

.. image:: images/nextflow-3.png
    :align: center
    :alt: launch a nextflow API app


**Step-3:** 

-  Uploading the necessary files, a GEM file in the format "\*.emx.txt" and a nextflow.config file (can upload all files at once). 
-  Click on "Upload" button. 

.. image:: images/nextflow-3.png
    :align: center
    :alt: launch a nextflow API app


**Step-4:** 

-  Now we are all set to launch the workflow. Go ahead and click on "Launch" button. 
-  This should show all the logs of the processes/jobs running in the background on the Kubernetes cluster.


.. image:: images/nextflow-4.png
    :align: center
    :alt: launch a nextflow API app

