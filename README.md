# SRE AI Assistant with OpenShift AI

This repository contains the details and assets needed to reproduce the SRE AI Assistant demo on OpenShift.

## Prerequisites

For the purposes of this demo, we deployed an Azure Red Hat OpenShift (ARO) cluster with the following characteristics:
Master Nodes: 3 master nodes of type Standard_D8s_v3, each with 1024 GB disk size, located in the northeurope region.


Worker Nodes: 3 worker nodes of type Standard_D4s_v3, each with 128 GB disk size, also in the northeurope region.


GPU Node: 1 worker node with GPU support, of type Standard_NC64as_T4_v3, with 128 GB disk size, configured for AI/ML workloads.


The cluster was configured with accelerated networking and uses Azure's premium managed disks for optimal performance.
This cluster was deployed specifically for this demo - however, any equivalent OpenShift cluster on any cloud provider or on-premises will work as long as it meets the aforementioned specifications. The most important part is the GPU instance where you will deploy your model meets the above specs.



## Setup

After provisioning an ARO cluster for the demo, we installed several key operators to make sure that the required functionality is there :
Red Hat - Authorino Operator (1.2.1 provided by Red Hat)
Red Hat OpenShift Logging (5.9.12 provided by Red Hat, Inc)
Elasticsearch (ECK) Operator (2.16.1 provided by Elastic)
OpenShift Elasticsearch Operator (5.8.19 provided by Red Hat)
NVIDIA GPU Operator (24.9.2 provided by NVIDIA Corporation)
Grafana Operator (5.17.0 provided by Grafana Labs)
OpenShift Lightspeed Operator (0.3.2 provided by Red Hat, Inc)
Node Feature Discovery Operator (4.16.0-202503121138 provided by Red Hat)
The NVIDIA NIM Operator for Kubernetes (1.0.1 provided by NVIDIA)
NVIDIA Network Operator (25.1.0 provided by NVIDIA)
Package Server (0.0.1-snapshot provided by Red Hat)
Red Hat OpenShift AI (2.18.0 provided by Red Hat, Inc)
Red Hat OpenShift Serverless (1.35.0 provided by Red Hat)
Red Hat OpenShift Service Mesh 2


Note : the versions are valid for the time of writing this document and tested on an ARO cluster version 4.16.30. 


We wanted to deploy an application that is the definition of a “ Microservices “ application with many moving parts so we went with the one described on this repo : https://github.com/quarkusio/quarkus-super-heroes . We deployed the application on a namespace called “ superheroes “ . In our case we deployed on an openshift cluster so all we had to do was run oc apply -f java21-openshift.yml and when the pods were all up , run the same for monitoring-openshift.yml , found under the quarkus-super-heroes
/deploy/k8s  directory 


For the purposes of  this demo we used ElasticSearch with Kibana for visualization. As long as the above operators are installed , its pretty easy to set it up, the sample log forwarder and instance yaml manifests are included in this repo. 
From the Openshift ElasticSearch Operator you need to create one ElasticSearch and               one Kibana instance and from the Red Hat OpenShift Logging you need to set up a Cluster Log Forwarder.




### LLM deployment

### The chat UI

### The broken Kafka instance

### The log view

1. Click on the OpenShift add-ons icon on the top right hand side of the OpenShift Console (square grid symbol). Select `Logging` to open the Kibana UI.
2. Select `Discover` in the left tool bar.
3. Select `Add a filter +` and set as field value `kubernetes.namespace_name is "superheroes"`.
4. Select `Add a filter +` and set as field value `level is "error"`.
5. Create an index 'app*' and then In the index dropdown, ensure that `app*` is selected.
6. Under `Available fields`, add the following:
    - `kubernetes.container_name`
    - `level`
    - `message`
7. Update the `Time Range` in the top right corner appropriately.

You should now be able to see the error messages of the broken Kafka pod.
