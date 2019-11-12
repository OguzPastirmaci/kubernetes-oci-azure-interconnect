# Kubeflow Deployment

The Kubeflow project is dedicated to making deployments of machine learning (ML) workflows on Kubernetes simple, portable and scalable. Our goal is not to recreate other services, but to provide a straightforward way to deploy best-of-breed open-source systems for ML to diverse infrastructures. Anywhere you are running Kubernetes, you should be able to run Kubeflow.

For more information on Kubeflow, visit [Kubeflow website](https://www.kubeflow.org/docs/started/getting-started/).

This guide creates a vanilla deployment of Kubeflow with all its core components without any external dependencies.

This Kubeflow deployment requires a default StorageClass with a dynamic volume provisioner. this guide uses [Local Path Provisioner](https://github.com/rancher/local-path-provisioner).

In this setup, the directory /opt/local-path-provisioner will be used across all the nodes as the path for provisioning (a.k.a, store the persistent volume data). The provisioner will be installed in local-path-storage namespace by default.

```console
kubectl apply -f https://raw.githubusercontent.com/rancher/local-path-provisioner/master/deploy/local-path-storage.yaml
```

After installation, you should see something like the following:

```console
$ kubectl -n local-path-storage get pod
NAME                                     READY     STATUS    RESTARTS   AGE
local-path-provisioner-d744ccf98-xfcbk   1/1       Running   0          7m
```

Create a hostPath backed Persistent Volume and a pod uses it:

```console
kubectl create -f https://raw.githubusercontent.com/rancher/local-path-provisioner/master/examples/pvc.yaml

kubectl create -f https://raw.githubusercontent.com/rancher/local-path-provisioner/master/examples/pod.yaml
```

You should see the PV has been created:

```console
$ kubectl get pv
NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS    CLAIM                    STORAGECLASS   REASON    AGE
pvc-bc3117d9-c6d3-11e8-b36d-7a42907dda78   2Gi        RWO            Delete           Bound     default/local-path-pvc   local-path               4s
```

The PVC has been bound:

```console
$ kubectl get pvc
NAME             STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
local-path-pvc   Bound     pvc-bc3117d9-c6d3-11e8-b36d-7a42907dda78   2Gi        RWO            local-path     16s
```

And the Pod started running:

```console
$ kubectl get pod
NAME          READY     STATUS    RESTARTS   AGE
volume-test   1/1       Running   0          3s
```

The pod was just for testing, so we can delete it:

```console
kubectl delete -f https://raw.githubusercontent.com/rancher/local-path-provisioner/master/examples/pod.yaml
```

We need to set this configuration as the default StorageClass:

```console
kubectl patch storageclass local-path -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
```

Download the kfctl v0.7.0 release from the Kubeflow releases page: (https://github.com/kubeflow/kubeflow/releases/tag/v0.7.0)

Unpack the tar ball:

```console
tar -xvf kfctl_v0.7.0_<platform>.tar.gz
```

Create environment variables to make the deployment process easier:

```sh
# The following command is optional. It adds the kfctl binary to your path.
# If you don't add kfctl to your path, you must use the full path
# each time you run kfctl.
# Use only alphanumeric characters or - in the directory name.
export PATH=$PATH:"<path-to-kfctl>"

# Set KF_NAME to the name of your Kubeflow deployment. You also use this
# value as directory name when creating your configuration directory.
# For example, your deployment name can be 'my-kubeflow' or 'kf-test'.
export KF_NAME=<your choice of name for the Kubeflow deployment>

# Set the path to the base directory where you want to store one or more 
# Kubeflow deployments. For example, /opt/.
# Then set the Kubeflow application directory for this deployment.
export BASE_DIR=<path to a base directory>
export KF_DIR=${BASE_DIR}/${KF_NAME}

# Set the configuration file to use when deploying Kubeflow.
# The following configuration installs Istio by default. Comment out 
# the Istio components in the config file to skip Istio installation. 
# See https://github.com/kubeflow/kubeflow/pull/3663
export CONFIG_URI="https://raw.githubusercontent.com/kubeflow/manifests/v0.7-branch/kfdef/kfctl_k8s_istio.0.7.0.yaml"
```

Notes:

* **${KF_NAME}** - The name of your Kubeflow deployment.
  If you want a custom deployment name, specify that name here.
  For example,  `my-kubeflow` or `kf-test`.
  The value of KF_NAME must consist of lower case alphanumeric characters or
  '-', and must start and end with an alphanumeric character.
  The value of this variable cannot be greater than 25 characters. It must
  contain just a name, not a directory path.
  You also use this value as directory name when creating the directory where 
  your Kubeflow  configurations are stored, that is, the Kubeflow application 
  directory. 

* **${KF_DIR}** - The full path to your Kubeflow application directory.

* **${CONFIG_URI}** - The GitHub address of the configuration YAML file that
  you want to use to deploy Kubeflow. The URI used in this guide is
  {{% config-uri-k8s-istio %}}.
  When you run `kfctl apply` or `kfctl build` (see the next step), kfctl creates
  a local version of the configuration YAML file which you can further
  customize if necessary.

To set up and deploy Kubeflow using the **default settings**,
run the `kfctl apply` command:

```
mkdir -p ${KF_DIR}
cd ${KF_DIR}
kfctl apply -V -f ${CONFIG_URI}
```

Check the resources deployed in namespace `kubeflow`:

```
kubectl -n kubeflow get all
```


