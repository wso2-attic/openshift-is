# Openshift resource for a Clustered Deployment of WSO2 Identity Server

## Contents

* [Prerequisites](#prerequisites)

* [Quick Start Guide](#quick-start-guide)

## Prerequisites


* WSO2 product Docker images used for the Openshift deployment.
  
  WSO2 product Docker images available at [DockerHub](https://hub.docker.com/u/wso2/) package General Availability (GA)
  versions of WSO2 products with no [WSO2 Updates](https://wso2.com/updates).

  For a production grade deployment of the desired WSO2 product-version, it is highly recommended to use the relevant
  Docker image which packages WSO2 Updates, available at [WSO2 Private Docker Registry](https://docker.wso2.com/). In order
  to use these images, you need an active [WSO2 Subscription](https://wso2.com/subscription).
  
* An already setup OpenShit cluster.

* Three NFS shares for storage. For step by step guide on how to install NFS server refer here. [CentOS](https://www.server-world.info/en/note?os=CentOS_7&p=nfs&f=1), [Ubuntu](https://www.server-world.info/en/note?os=Ubuntu_18.04&p=nfs&f=1)

## Quick Start Guide

1. Create a OpenShift project name `wso2`.

   `oc new-project wso2`

2. Add your subscription details.

   `oc create secret docker-registry wso2-image-pull-secret --docker-server=docker.wso2.com --docker-username=<SUBSCRIPTION_USERNAME> --docker-password=<SUBSCRIPTION_PASSWORD>`

3. Deploy WSO2 Identity Server HA cluster.

   `oc process -f Template.yaml -p NFS_SERVER_IP=<NFS_SERVER_IP> -p NFS_SHARE_DATABASE=<DATABASE_NFS_SHARE_PATH> -p NFS_SHARE_USERSTORE=<USERSTORE_NFS_SHARE_PATH> -p NFS_SHARE_TENANT=<TENANT_SHARE_PATH> | oc create -f -`

4. Access product management consoles.
   Obtain the (HOST/PORT) of the route resource.
   
   `oc get route -n <PROJECT_NAME>`
   
   Try navigating to `https://<HOST:PORT>` from your favorite browser.

## Configuration

The following tables lists the configurable parameters of the template and their default values.

| Parameter                                                                   | Description                                                                               | Default Value               |
|-----------------------------------------------------------------------------|-------------------------------------------------------------------------------------------|-----------------------------|
|`OPENSHIFT_PROJECT_NAME`|Name of the OpenShift project|wso2|
|`NAME`|Name of the deployment|wso2is|
|`REPLICAS`|Number of replicas|2|
|`WSO2_IS_IMAGE_NAME`|WSO2 Identity Server image name|docker.wso2.com/wso2is:5.10.0|
|`IMAGE_PULL_SECRET`|WSO2 Subscription Credentials|wso2-image-pull-secret|
|`NFS_SERVER_IP`|NFS Server IP|`required`|
|`NFS_SHARE_DATABASE`|NFS Share Path for database|`required`|
|`NFS_SHARE_USERSTORE`|NFS Share Path for userstore|`required`|
|`NFS_SHARE_TENANT`|NFS Share Path for tenant|`required`|
|`RESOURCES_LIMITS_CPU`|Set a CPU resource limits|4000m|
|`RESOURCES_LIMITS_MEMORY`|Set a memory resource limits|4Gi|
|`RESOURCES_REQUEST_CPU`|Set a minimum CPU resource limits|2000m|
|`RESOURCES_REQUEST_MEMORY`|Set a minimum memory resource limits|2Gi|
|`WSO2_IS_SERVICE_ACCOUNT_NAME`|Service Name|wso2svc-account|