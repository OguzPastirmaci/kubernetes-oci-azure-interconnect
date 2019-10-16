1- Enable the ol7_addons repository on each system in your deployment

`yum-config-manager --enable ol7_addons`

2- Setup UEK R5

`yum-config-manager --disable ol7_UEKR4`

`yum-config-manager --enable ol7_UEKR5`

`yum update`

`systemctl reboot`

3- Install Docker Engine

`yum install docker-engine`

`systemctl enable docker`

`systemctl start docker`

`sudo usermod -aG docker $USER`

4- Make sure you can access Oracle Container Registry

Log in to the Oracle Container Registry website at https://container-registry.oracle.com using your Single Sign-On credentials.

Use the web interface to navigate to the Container Services business area and accept the Oracle Standard Terms and Restrictions for the Oracle software images that you intend to deploy. You are able to accept a global agreement that applies to all of the existing repositories within this business area. If newer repositories are added to this business area in the future, you may need to accept these terms again before performing upgrades.

`docker login container-registry.oracle.com`

5- Configure firewall

`iptables -P FORWARD ACCEPT`

`firewall-cmd --add-masquerade --permanent`

`firewall-cmd --add-port=10250/tcp --permanent`

`firewall-cmd --add-port=8472/udp --permanent`

Additionally, run the following command on the master node:

`firewall-cmd --add-port=6443/tcp --permanent`

`systemctl restart firewalld`

5- Disable SELinux

`/usr/sbin/setenforce 0`

