# istio-workshop

## Installing Redhat Service Mesh
This is an extract from Redhat official document. A quick way to get service mesh up and runnning. For more detail document please refer to
[Installing Openshift Service Mesh](https://docs.openshift.com/container-platform/4.7/service_mesh/v2x/ossm-about.html)

## Install Elastic Search

1. Navigate to Operators --> OperatorHub.
2. Type Elasticsearch into the filter box to locate the OpenShift Elasticsearch Operator.
3. Click the OpenShift Elasticsearch Operator provided by Red Hat to display information about the Operator.

   ![Elasticsearch](https://github.com/rh-telco-tigers/istio-workshop/blob/main/images/elasticsearch.png)

4. Click Install.

   ![Elastichsearch Install](https://github.com/rh-telco-tigers/istio-workshop/blob/main/images/es-install.png)

5. On the Install Operator page, under Installation Mode select All namespaces on the cluster (default). This makes the Operator available to all projects in the cluster.
6. Under Installed Namespaces select openshift-operators-redhat from the menu.
7. Select the Update Channel that matches your OpenShift Container Platform installation. For example, if you are installing on OpenShift Container Platform version 4.6, select the 4.6 update channel.
8. Select the Automatic Approval Strategy.
9. Click Install.

   ![Operator Install](https://github.com/rh-telco-tigers/istio-workshop/blob/main/images/operator-install.png)

10. On the Installed Operators page, select the openshift-operators-redhat project. Wait until you see that the OpenShift Elasticsearch Operator shows a status of "InstallSucceeded" before continuing.

   ![Install Success](https://github.com/rh-telco-tigers/istio-workshop/blob/main/images/install-success.png)
    
## Installing and Configuring Redhat Cluster Logging

1. Navigate to Operators → OperatorHub.
2. Type logging into the filter to locate the logging Operator.

   ![Logging Opeartor](https://github.com/rh-telco-tigers/istio-workshop/blob/main/images/logging-install.png)

3. Click the loging Operator provided by Red Hat to display information about the Operator.
4. Click Install.

   ![Logging Install](https://github.com/rh-telco-tigers/istio-workshop/blob/main/images/logging-operator.png)

## Creating Cluster logging instance

1. Navigate to Administration --> CustomResourceDefination
2. search for ClusterLogging
3. Click on instances (you may have to refresh the page).
4. Click on create instance

   ![Logging Instance](https://github.com/rh-telco-tigers/istio-workshop/blob/main/images/logging-instance.png)

5. Paste the following yaml and change the storage class name (persistance Storage) and click on create

``` yaml
apiVersion: "logging.openshift.io/v1"
kind: "ClusterLogging"
metadata:
  name: "instance" 
  namespace: "openshift-logging"
spec:
  managementState: "Managed"  
  logStore:
    type: "elasticsearch"  
    retentionPolicy: 
      application:
        maxAge: 1d
      infra:
        maxAge: 7d
      audit:
        maxAge: 7d
    elasticsearch:
      nodeCount: 3 
      storage:
        storageClassName: "<storage-class-name>"   # <--- Chnage storageclass name
        size: 100G
        resources: 
          limits:
            memory: "4Gi"
          requests:
            memory: "4Gi"            
        proxy: 
          limits:
            memory: 256Mi
          requests:
             memory: 256Mi
      redundancyPolicy: "SingleRedundancy"
  visualization:
    type: "kibana"  
    kibana:
      replicas: 1
  curation:
    type: "curator"
    curator:
      schedule: "30 3 * * *" 
  collection:
    logs:
      type: "fluentd"  
      fluentd: {}   
```
6. Navogate to projects --> openshift-logging
7. You will see that elastciserach pods being created

   ![elsaticsearch pods](https://github.com/rh-telco-tigers/istio-workshop/blob/main/images/pod-create.png)
   
8. After few minutes your pods in openshift-logging project should look like below.

   ![elastic pods](https://github.com/rh-telco-tigers/istio-workshop/blob/main/images/elastic-pods.png)
   
## Installing the Jeager Operator

To install Jaeger you use the OperatorHub to install the Jaeger Operator. By default the Operator is installed in the openshift-operators project.

## Prerequisites

1. Access to the OpenShift Container Platform web console.
2. An account with the cluster-admin role.
3. If you require persistent storage, you must also install the OpenShift Elasticsearch Operator before installing the Jaeger Operator.

## Procedure

1. Navigate to Operators → OperatorHub.
2. Type Jaeger into the filter to locate the Jaeger Operator.
3. Click the Jaeger Operator provided by Red Hat to display information about the Operator.
4. Click Install.

   ![Jaeger Install](https://github.com/rh-telco-tigers/istio-workshop/blob/main/images/jaeger-op.png)

5. On the Install Operator page, select the stable Update Channel. This will automatically update Jaeger as new versions are released. If you select a maintenance channel, for example, 1.17-stable, you will receive bug fixes and security patches for the length of the support cycle for that version.
6. Select All namespaces on the cluster (default). This installs the Operator in the default openshift-operators project and makes the Operator available to all projects in the cluster.
7. Click Install.

   ![Jaeger Install](https://github.com/rh-telco-tigers/istio-workshop/blob/main/images/jaeger-install.png)

8. On the Subscription Overview page, select the openshift-operators project. Wait until you see that the Jaeger Operator shows a status of "InstallSucceeded" before continuing.

   ![Jaeger Succeed](https://github.com/rh-telco-tigers/istio-workshop/blob/main/images/jaeger-install-suceed.png)

## Installing Kiali

1. Navigate to Operators → OperatorHub.
2. Type Kiali into the filter box to find the Kiali Operator.
3. Click the Kiali Operator provided by Red Hat to display information about the Operator.
4. Click Install.

   ![Kiali Operatopr](https://github.com/rh-telco-tigers/istio-workshop/blob/main/images/kiali-op.png)

5. On the Operator Installation page, select the stable Update Channel.
6. Select All namespaces on the cluster (default). This installs the Operator in the default openshift-operators project and makes the Operator available to all projects in the cluster.
7. Select the Automatic Approval Strategy.
8. Click Install.

   ![Kiali Install](https://github.com/rh-telco-tigers/istio-workshop/blob/main/images/kiali-install.png)

9. The Installed Operators page displays the Kiali Operator’s installation progress. Wait until you see that the Kiali Operator shows a status of "InstallSucceeded" before continuing.
   
   ![Kiali Installed](https://github.com/rh-telco-tigers/istio-workshop/blob/main/images/kiali-install-suceed.png)

## Installing the Red Hat OpenShift Service Mesh Operator

1. Navigate to Operators → OperatorHub.
2. Type Red Hat OpenShift Service Mesh into the filter box to find the Red Hat OpenShift Service Mesh Operator.
3. Click the Red Hat OpenShift Service Mesh Operator to display information about the Operator.
4. Click Install.

   ![SM Operator](https://github.com/rh-telco-tigers/istio-workshop/blob/main/images/sm-operator.png)

5. On the Operator Installation page, select the stable Update Channel.
6. In the Installation Mode section, select All namespaces on the cluster (default). This installs the Operator in the default openshift-operators project and makes the Operator available to all projects in the cluster.
7. Select the Automatic Approval Strategy.
8. Click Install

   ![sm-install](https://github.com/rh-telco-tigers/istio-workshop/blob/main/images/sm-install.png)

9. The Installed Operators page displays the ServiceMesh Operator’s installation progress. Wait until you see that the ServiceMesh Operator shows a status of "InstallSucceeded" before continuing.
 
   ![sm-suceed](https://github.com/rh-telco-tigers/istio-workshop/blob/main/images/sm-succeed.png)
   
### Deploying the control plane from the web console

1. Create a project named istio-system.
      a. Navigate to Home → Projects.
      b. Click Create Project.
      c. Enter istio-system in the Name field.
      d. Click Create.

2. Navigate to Operators → Installed Operators.
3. In the Project menu, select istio-system. You may have to wait a few moments for the Operators to be copied to the new project.
4. Click the Red Hat OpenShift Service Mesh Operator, then click Istio Service Mesh Control Plane.

   ![istio-cp](https://github.com/rh-telco-tigers/istio-workshop/blob/main/images/istio-cp.png)

5. On the Istio Service Mesh Control Plane page, click Create ServiceMeshControlPlane.

   ![istio-cp-crate](https://github.com/rh-telco-tigers/istio-workshop/blob/main/images/istio-cp-create.png)

6. On the Create ServiceMeshControlPlane page, you can modify the default ServiceMeshControlPlane template with the form, or select the YAML view to customize your installation.
7. Click Create to create the control plane. The Operator creates pods, services, and Service Mesh control plane components based on your configuration parameters.
8. Click the Istio Service Mesh Control Plane tab.
9. Click the name of the new control plane.
10. Click the Resources tab to see the Red Hat OpenShift Service Mesh control plane resources the Operator created and configured.
11. Click on istio-system project you will see pods running there

    ![Istio Complete](https://github.com/rh-telco-tigers/istio-workshop/blob/main/images/istio-complete.png)
    
# Appendix
Configure cluster logging instance without persistent storage. Incase persistent storage is not available in your environment you can use following yaml template to create instance without presitent storage

```yaml
apiVersion: "logging.openshift.io/v1"
kind: "ClusterLogging"
metadata:
  name: "instance" 
  namespace: "openshift-logging"
spec:
  managementState: "Managed"  
  logStore:
    type: "elasticsearch"  
    retentionPolicy: 
      application:
        maxAge: 1d
      infra:
        maxAge: 7d
      audit:
        maxAge: 7d
    elasticsearch:
      nodeCount: 3 
      storage: {}.  # <--- no storage
  visualization:
    type: "kibana"  
    kibana:
      replicas: 1
  curation:
    type: "curator"
    curator:
      schedule: "30 3 * * *" 
  collection:
    logs:
      type: "fluentd"  
      fluentd: {}
```      
