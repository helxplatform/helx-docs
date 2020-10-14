###########################
Current & Upcoming Releases
###########################

HeLx is alpha. This section outlines a few areas of anticipated focus
for upcoming improvements.

*************
Architecture
*************

**Semantic Search** 

Our team has a `semantic search
engine_ for mapping to research
data sets based on full text search. We anticipate extending the Tycho
metadata model to include semantic links to ontologies, incorporating
analytical tools into the semantic search capability. See
EDAM_ &
Biolink_ for more
information.

.. _engine: https://github.com/helxplatform/dug
.. _EDAM: http://edamontology.org/page
.. _Biolink: https://biolink.github.io/biolink-model/

**Utilization Metrics** 

Basic per application resource utilization
information is already provided. But we anticipate creating scalable
policy based resource management able to cope with the range of
implications of the analytic workspaces we provide, ranging from single
user notebooks, to GPU accelerated workflows, to Spark clusters.

**Proxy Management** 

Coming soon.

**Helm 3 Deployment** 

Coming soon.

*********************
Current Deployments
*********************

+-------------------+-----------------------------------------------------+---------------+-----------------+
| **Application**   | **Version**                                         | **Cluster**   | **Namespace**   |
+===================+=====================================================+===============+=================+
| ambassador        | quay.io/datawire/ambassador:1.1.1                   | helx-prod     | helx            |
+-------------------+-----------------------------------------------------+---------------+-----------------+
| cs-appstore       | heliumdatastage/appstore:masterccav0.0.16           | helx-prod     | helx            |
+-------------------+-----------------------------------------------------+---------------+-----------------+
| nfs-server        | gcr.io/google\_containers/volume-nfs:0.8            | helx-prod     | helx            |
+-------------------+-----------------------------------------------------+---------------+-----------------+
| nginx-revproxy    | heliumdatastage/nginx:cca-v0.0.5                    | helx-prod     | helx            |
+-------------------+-----------------------------------------------------+---------------+-----------------+
| tycho-api         | heliumdatastage/tycho-api:masterccav0.0.10          | helx-prod     | helx            |
+-------------------+-----------------------------------------------------+---------------+-----------------+
| ambassador        | quay.io/datawire/ambassador:1.1.1                   | helx-val      | default         |
+-------------------+-----------------------------------------------------+---------------+-----------------+
| nginxrevproxy     | heliumdatastage/nginx:cca-v0.0.5                    | helx-val      | default         |
+-------------------+-----------------------------------------------------+---------------+-----------------+
| ambassador        | quay.io/datawire/ambassador:1.1.1                   | helx-dev      | helx-dev        |
+-------------------+-----------------------------------------------------+---------------+-----------------+
| cs-appstore       | heliumdatastage/appstore:masterccav0.0.18helx-dev   | helx-dev      | helx-dev        |
+-------------------+-----------------------------------------------------+---------------+-----------------+
| nfs-server        | gcr.io/google\_containers/volume-nfs:0.8            | helx-dev      | helx-dev        |
+-------------------+-----------------------------------------------------+---------------+-----------------+
| nginx-revproxy    | heliumdatastage/nginx:cca-v0.0.5                    | helx-dev      | helx-dev        |
+-------------------+-----------------------------------------------------+---------------+-----------------+
| tycho-api         | heliumdatastage/tycho-api:masterccav0.0.12          | helx-dev      | helx-dev        |
+-------------------+-----------------------------------------------------+---------------+-----------------+
| ambassador        | quay.io/datawire/ambassador:1.1.1                   | scidas-dev    | default         |
+-------------------+-----------------------------------------------------+---------------+-----------------+
| cs-appstore       | heliumdatastage/appstore:mastercca-v0.0.5           | scidas-dev    | default         |
+-------------------+-----------------------------------------------------+---------------+-----------------+
| nfs-server        | gcr.io/google\_containers/volume-nfs:0.8            | scidas-dev    | default         |
+-------------------+-----------------------------------------------------+---------------+-----------------+

