# Step 4: Deploying a Kubernetes cluster

1. Add Kubernetes Signing Key
   
```console
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add
```
2. Add Software Repositories
   
```console
sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
```

3. Install Kubernetes tools with the following command. Kubeflow recommends using Kubernetes v1.14, so we're pinning the versions of the tools to 1.14. 

**IMPORTANT:** Repeat this step for each Kubernetes node (both master and workers)

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

4. Switch to the `master` node and run the following command. This will initialize our cluster:

```console
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
```

Once this command finishes, it will display a `kubeadm join` command at the end similar to following:

```console
kubeadm join 10.0.2.24:6443 --token <some token> \
    --discovery-token-ca-cert-hash sha256:<some hash>
```

Make a note of it, we will use it for joining worker nodes to the master.


5. While still on the `master`, let's create the kubeconfig for running `kubectl` commands against our cluster.

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
$ kubectl get nodes

NAME               STATUS   ROLES    AGE    VERSION
oci-k8s-master     Ready    master   1m   v1.14.8
```

6. In this step, we will deploy a pod network. A pod network enables communication between nodes in the cluster. We will use [`flannel`](https://github.com/coreos/flannel) as the pod network. Flannel is a very simple overlay network that satisfies the Kubernetes requirements.

```console
sudo kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

Verify that the pods are running. You should see the status of the pods as `Running`.

```console
kubectl get pods --all-namespaces
```

We have the Kubernetes master node and the pod network running. It's time to join worker nodes to the master.

7. Switch to your worker node and run the `kubeadm join` command similar to below.

**IMPORTANT:** Your command will have different values, do not copy and paste the below command. Use the command that was displayed after you ran `sudo kubeadm init --pod-network-cidr=10.244.0.0/16`.

```console
kubeadm join 10.0.2.24:6443 --token <some token> \
    --discovery-token-ca-cert-hash sha256:<some hash>
```

Run the `kubeadm join` command in all of your worker nodes.

8. Switch to master and run `kubectl get nodes` to check if all worker nodes are added:

```console
$ kubectl get nodes

NAME               STATUS   ROLES    AGE    VERSION
azure-k8s-worker   Ready    <none>   13m   v1.14.8
oci-k8s-master     Ready    master   12m   v1.14.8
oci-k8s-worker     Ready    <none>   13m   v1.14.8
```
# Next Step

After you finish deploying the Kubernetes cluster, continue to [Step 5: Deploying Kubeflow](../docs/kubeflow-setup.md).