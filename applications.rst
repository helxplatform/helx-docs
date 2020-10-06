############
Applications
############

Appstore is deployed to Kubernetes in production using Helm. The main
deployment concerns are: 

- **Security** : Secrets are added to the container via environment variables. 

- **Persistence** : Storage must be mounted for a database. 

- **Services** : The chief dependency is on Tycho which must be at the correct version.

The Helm chart for deploying Appstore can be found
here_.

.. _here: https://github.com/helxplatform/devops/tree/master/helx/charts/appstore


.. Hide the contents from the front page because they are already in
.. the side bar in the Alabaster sphinx style; requires Alabaster
.. config sidebar_includehidden=True (default)

.. Contents
.. ========

.. toctree::
   :maxdepth: 4
   :numbered:		
   :hidden:
     
   Creating an Application <app_create>
   Blackbalsam <app_blackbalsam>
   Cloudtop <app_cloudtop>
   DICOM Viewer for Google Health API <app_dicom>
   ImageJ-Napari <app_imagej_napari>
   Jupyter-DataScience <app_jupyter_datascience>
   Nextflow API <app_nextflow_api>
   RStudio <app_rstudio>

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
