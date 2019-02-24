# Introduction 
Using EFK stack in kubernetes on windows

# Getting Started
I used some code from other places, will just give links (rather than submodules & symlinks) due to having symlink problems in the past.

## Prerequisites
*Ensure you are using a rs_prelease `18239` or higher build*

## Steps
1. Compile all Docker files for efk stack (assumes microsoft/windowservercore:latest is correctly tagged)
```
# you may not want to build fluentd with hyperv, but I found it more reliably succeeded
docker build https://github.com/KnicKnic/fluentd-docker-image.git#kube_image:v1.2/windows -t fluentd:1 --isolation=hyperv
Docker tag fluentd:1 fluentd
docker build https://github.com/KnicKnic/dockerfiles-windows.git#use_latest:openjdk/windowsservercore -t openjdk:1
docker tag openjdk:1 openjdk
docker build https://github.com/KnicKnic/dockerfiles-windows.git#use_latest:elasticsearch/windowsservercore/latest -t elasticsearch:1
docker tag elasticsearch:1 elasticsearch
docker build https://github.com/KnicKnic/dockerfiles-windows.git#use_latest:kibana/windowsservercore -t kibana:1
docker tag kibana:1 kibana
docker build https://github.com/KnicKnic/kubernetes.git#fluentd:cluster/addons/fluentd-elasticsearch/fluentd-es-image -t fluentd-kube:1
docker tag fluentd-kube:1 fluentd-kube
```
2. add kubernetes files
    1. https://github.com/KnicKnic/kubernetes/tree/fluentd/cluster/addons/fluentd-elasticsearch
(pay attention to the last 2 code edits as I hard coded some variables to my environment (due to things being broken) and you probably do not want that)
        1. I will eventually go back and fix them when I have time to verify the edits, but untill then I am not doing so as those are what I tested with.

## Other notables
This code makes use of the [kubernetes metadata filter for fluentd](https://github.com/fabric8io/fluent-plugin-kubernetes_metadata_filter)
