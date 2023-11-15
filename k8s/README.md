# Manual Deployment of Plato on Kubernetes

Deploy a self-hosted Plato on Kubernetes clusters using manually configured manifests.

# Prerequisites

For self-hosted Plato deployment, you need:

    - A Plato license key from your Plato representative.
    - A domain for adding a DNS record.
    - A Kubernetes cluster. Refer to Google Cloud Platform, AWS, and Azure documentation for cluster creation.
    - A functional kubectl installation. Refer to Google Cloud Platform, AWS, and Azure documentation for installation.

# Cluster Specifications

The cluster should have at least one node with 2x vCPUs and 8 GB memory. Use the following command to check your nodes' capacity.

```
$ kubectl describe nodes
```

# Cluster Storage Class

Ensure your cloud provider's volume can be mounted to multiple nodes.
Run `kubectl get storageclass` to identify your cluster's storage class.
Confirm if this storage class supports the ReadWriteMany access mode from your cloud provider's documentation.

# Obtaining Manifests

Self-hosted Plato deployments use a set of manifests.
Contact your Plato representative to get a copy of the manifests.
Open the kubernetes directory in an IDE to follow the steps below.

```
unzip plato-k8s-manifests.zip
cd plato-k8s-manifests
```

# Version Configuration (Optional)

In `plato-service-api.yaml`, change the image tag to specify the Plato version to install.
Use `latest` to always refer to the most recent Plato version.
The example below specifies the image tag for version `1.2023-11-14-2125-3124a01`.

```
image: us-west2-docker.pkg.dev/rover-prod-9acae/on-prem/plato:1.2023-11-14-2125-3124a01
```

# Configuration Update

Copy `plato-secrets.template.yaml` to a new file named `plato-secrets.yaml`.
This file sets the deployment configuration options and stores them as Kubernetes secrets.

```
cp plato-secrets.template.yaml plato-secrets.yaml
```

Set your instance's configuration options. Note that values in this file must be base64 encoded.

| Setting             | Description                                                                                                       |
| -------             | -----------                                                                                                       |
| `postgres_password` | Plato's internal database password. Generate a base64 encoded string with openssl.                                |
| `db_creds_enc_key`  | Key for encrypting the database credentials. Generate a base64 encoded string with openssl.                       |
| `license_key`       | Plato's internal database password. Get a license key from your representative and encode the string into base64. |

# Apply Manifest Changes

```
kubectl apply -f plato-namespace.yaml
kubectl apply -f plato-secrets.yaml
kubectl apply -f plato-service-postgres.yaml
kubectl apply -f plato-service-api.yaml
```

# Verify Pod Status

```
$ kubectl get pods -n plato

NAME                       READY   STATUS    RESTARTS   AGE
api-86478745f4-8gt47       1/1     Running   0          28s
postgres-c6586d59d-r44nf   1/1     Running   0          11m
```
