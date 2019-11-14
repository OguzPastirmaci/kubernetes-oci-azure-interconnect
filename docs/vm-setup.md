# Configuring the virtual machines

Follow the steps below for configuring the VMs you deployed in the [previous step](../docs/vm-deployment.md).

## Disable swap
Swap needs to be disabled  in order for the kubelet to work properly.

```console
sudo swapoff -a
```

```console
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
```

## Install Docker 18.09
Currently, the latest Docker version is 19.03. This guide deploys Docker 18.09.

In the later steps, we will deploy Kubeflow to our Kubernetes cluster. Kubeflow recommends Kubernetes v1.14 and Kubernetes v1.14 does not support the new `--gpus` flag that comes with Docker 19.03.

Another reason is that Kubeadm 1.14 is validated for Docker 18.09.


```console
sudo apt-get update
```

```console
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```

```console
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

```console
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

```console
sudo apt-get update
```

```console
sudo apt-get install docker-ce=5:18.09.9~3-0~ubuntu-bionic docker-ce-cli=5:18.09.9~3-0~ubuntu-bionic containerd.io
```

```console
sudo systemctl enable docker
```

## Install Nvidia drivers
If you have any nodes that have GPUs, you need to install the Nvidia drivers. If you are not planning to have any GPU nodes, you may skip this step.

```console
sudo apt update
```
```console
sudo apt install ubuntu-drivers-common
```

```console
sudo ubuntu-drivers autoinstall
```


## Install nvidia-docker2
If you have any nodes that have GPUs, you need to install the `nvidia-docker2`. If you are not planning to have any GPU nodes, you may skip this step.

```console
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
```

```console
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
```

```console
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
```

```console
sudo apt-get update && sudo apt-get install -y nvidia-docker2
```

```console
sudo systemctl restart docker
```

You will need to enable the nvidia runtime as your default runtime on your node. We will be editing the docker daemon config file which is usually present at `/etc/docker/daemon.json`:

Use an editor (nano, vi etc.) to change the content of the `/etc/docker/daemon.json` file to the following:

```sh
{
    "default-runtime": "nvidia",
    "runtimes": {
        "nvidia": {
            "path": "/usr/bin/nvidia-container-runtime",
            "runtimeArgs": []
        }
    }
}
```

# Next Step

After you finish configuring the VMs, continue to [deploying a Kubernetes cluster](../docs/kubernetes-setup.md).