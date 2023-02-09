---
title: Installing Prometheus in a Kubernetes Cluster
date: 2023-02-09T19:34:30-04:00
categories:
  - Blog
author: George Farcas
tags: [prometheus, linux, k8s]
toc: true
toc_label: "Content"
---
### Prometheus is an open-source monitoring and alerting system widely used in cloud native environments. In this article, we will cover the steps to install Prometheus in a Kubernetes cluster.

### Prerequisites
A running Kubernetes cluster
kubectl installed on your machine and configured to communicate with your cluster
Familiarity with the Kubernetes namespace and YAML files

### Step 1: Create a Namespace
To install Prometheus, we need to create a new namespace in our Kubernetes cluster to keep the resources separate.

```shell
kubectl create namespace prometheus
```
Step 2: Install the Prometheus Operator

The Prometheus Operator is a tool that makes it easy to manage and operate Prometheus clusters on top of Kubernetes. To install the Prometheus Operator, run the following command:

```shell
kubectl apply -f https://raw.githubusercontent.com/prometheus-operator/prometheus-operator/main/bundle.yaml -n prometheus
```

### Step 3: Create a Prometheus Deployment
Now that we have installed the Prometheus Operator, we can create a Prometheus deployment in our cluster. This is done by creating a Prometheus custom resource definition (CRD).

Here is an example Prometheus CRD:

```yaml
apiVersion: monitoring.coreos.com/v1
kind: Prometheus
metadata:
  name: example
  namespace: prometheus
spec:
  version: v2.22.1
  serviceAccountName: example-prometheus
  ruleSelector:
    matchLabels:
      prometheus: example
  resources:
    requests:
      memory: 400Mi
  replicas: 2
  retention: 30d
  serviceMonitorSelector:
    matchLabels:
      app: example
```

Apply the CRD using the following command:

```shell
kubectl apply -f prometheus.yaml -n prometheus
```

Step 4: Verify the Installation
Once the Prometheus deployment is created, we can verify the installation by checking the pods in the prometheus namespace.

```shell
kubectl get pods -n prometheus
```

You should see two Prometheus pods in the Running state.

Step 5: Access the Prometheus Web UI
To access the Prometheus web UI, we need to expose the Prometheus service using a Kubernetes LoadBalancer. Here is an example service definition:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: example-prometheus
  namespace: prometheus
  labels:
    prometheus: example
spec:
  selector:
    prometheus: example
  ports:
    - name: web
      port: 9090
      targetPort: web
  type: LoadBalancer
```

Apply the service definition using the following command:

```shell
kubectl apply -f prometheus-service.yaml -n prometheus
```

Once the service is created, you can access the Prometheus web UI by using the LoadBalancer IP address and port 9090.

In this article, we covered the steps to install Prometheus in a Kubernetes cluster. With Prometheus, you can monitor your applications and infrastructure and receive alerts in case of any issues.
