# Deploying a Kubernetes cluster with Kubeadm


```console
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add
```

```console
sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
```

Install Kubernetes tools with the following command. Kubeflow recommends using Kubernetes v1.14, so we're pinning the versions of the tools to 1.14. 

```console
sudo apt-get install kubeadm=1.14.8-00 kubelet=1.14.8-00 kubectl=1.14.8-00
```

```console
sudo apt-mark hold kubeadm kubelet kubectl
```

Verify the installation.

```console
kubeadm version
```

Switch to the `master` node and run the following command:

```console
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
```

Once this command finishes, it will display a `kubeadm join` command at the end similar to following:

```console
kubeadm join 10.0.2.24:6443 --token a133ev.kxr3kwcbmc8a840m \
    --discovery-token-ca-cert-hash sha256:f41ea9f0ff32589919cb79634d4371de7c1f5bdd15e11c96c30e4f0d407ea147
```

Make a note of it, we will use it for joining worker nodes to the master.

Now let's create the kubeconfig for running `kubectl` commands against our cluster.

```console
mkdir -p $HOME/.kube
```

```console
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
```

```console
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

Now run the following command, you should see your master listed.

```console
kubectl get nodes
```

```console
ubuntu@oci-k8s-master: kubectl get nodes

NAME               STATUS   ROLES    AGE    VERSION
oci-k8s-master     Ready    master   1m   v1.14.8
```

A pod network enables communication between nodes in the cluster. We will use [`flannel`](https://github.com/coreos/flannel) as the pod network. Flannel is a very simple overlay network that satisfies the Kubernetes requirements.

```console
sudo kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

Verify that the pods are running

```console
kubectl get pods --all-namespaces
```

We have the Kubernetes master node and the pod network running. It's time to join worker nodes to the master.

Switch to your worker node and run the `kubeadm join` command similar to below.

**IMPORTANT:** Your command will have different values, do not copy and paste the below command. Use the command that was displayed after you ran `sudo kubeadm init --pod-network-cidr=10.244.0.0/16`.

```console
kubeadm join 10.0.2.24:6443 --token a133ev.kxr3kwcbmc8a840m \
    --discovery-token-ca-cert-hash sha256:f41ea9f0ff32589919cb79634d4371de7c1f5bdd15e11c96c30e4f0d407ea147
```

Run the `kubeadm join` command in all of your worker nodes. Then switch to master and run `kubectl get nodes` to check if all worker nodes are added:

```console
ubuntu@oci-k8s-master:$ kubectl get nodes

NAME               STATUS   ROLES    AGE    VERSION
azure-k8s-worker   Ready    <none>   13m   v1.14.8
oci-k8s-master     Ready    master   12m   v1.14.8
oci-k8s-worker     Ready    <none>   13m   v1.14.8
```