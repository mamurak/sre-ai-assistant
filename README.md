# SRE AI Assistant with OpenShift AI

This repository contains the details and assets needed to reproduce the SRE AI Assistant demo on OpenShift.

## Prerequisites

## Setup

### LLM deployment

### The chat UI

### The broken Kafka instance

### The log view

1. Click on the OpenShift add-ons icon on the top right hand side of the OpenShift Console (square grid symbol). Select `Logging` to open the Kibana UI.
2. Select `Discover` in the left tool bar.
3. Select `Add a filter +` and set as field value `kubernetes.namespace_name is "superheroes"`.
4. Select `Add a filter +` and set as field value `level is "error"`.
5. In the index dropdown, ensure that `app*` is selected.
6. Under `Available fields`, add the following:
    - `kubernetes.container_name`
    - `level`
    - `message`
7. Update the `Time Range` in the top right corner appropriately.

You should now be able to see the error messages of the broken Kafka pod.