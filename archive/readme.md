Short version
================

1’.
```sh
nano /etc/hostname
```
Change the hostname and then press ‘Ctrl+X’, then press ‘Y’ and then press ‘Enter’ to Save the file.

Reboot the machines.
```sh
reboot
```

### Install Kubernetes Components

Perform the following steps on both the master and node machines.

Add Kubernetes signing key.
```sh
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
```

Add Kubernetes repositories.
```sh
echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" | tee /etc/apt/sources.list.d/kubernetes.list
```

Update the packages list and install Kubernetes.
```sh
apt-get update
apt-get install -y kubelet kubeadm kubectl
```

Disable automatic updates.
```sh
apt-mark hold kubelet kubeadm kubectl
```

### Initialize Kubernetes Master

Perform the following steps only on the master machine.

Initialize Kubernetes master.
```sh
kubeadm init --pod-network-cidr=10.244.0.0/16
```

After the command finishes, note down the `kubeadm join` command. You'll need it to join the nodes to the master later.

Exit the root user.
```sh
exit
```

Set up the local kubeconfig for the user.
```sh
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

### Set up Pod Network

Perform the following steps only on the master machine.

Install the Flannel pod network.
```sh
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

### Join Nodes to the Master

Perform the following steps on all node machines.

Join the node to the master using the `kubeadm join` command you noted down earlier. The command should look like this (with the actual values from your master machine):
```sh
kubeadm join --token <token> <master-ip>:<master-port> --discovery-token-ca-cert-hash sha256:<hash>
```

### Verify the Cluster

Perform the following steps only on the master machine.

Check the nodes in the cluster.
```sh
kubectl get nodes
```

You should see all your nodes, including the master, with the status `Ready`.

Congratulations! You have successfully set up a Kubernetes cluster with one master and multiple nodes.



Kubernetes lab, K8s Spark Setup
================================ 

## Download Resources
- [Oracle Virtual Box](https://www.virtualbox.org/wiki/Downloads)
- [Ubuntu 18.10](http://releases.ubuntu.com/)

## Oracle Virtual Machines Setup

### Oracle Virtual Box Ubuntu Installation
1. Install Oracle Virtual Box on your host system.
2. Create a new virtual machine by following the provided screenshots as a visual guide.

Once you have completed the installation, shut down the VM by closing the window, without saving. Eject the installer ISO from the VM's CD drive.

Clone the VM "ubuntu1" two times and name the clones "ubuntu2" and "ubuntu3".

### Virtual Machine Network Settings
On each virtual machine (ubuntu1, ubuntu2, and ubuntu3), check the network settings. Provide a NAT adapter and a host-only adapter. Set Adapter 2 to "Host-only Adapter".

### Virtual Machine Initial Setup
Perform the following steps for ubuntu1, ubuntu2, and ubuntu3:
1. Start the virtual machine.
2. Skip the setup screens and confirm to install the updates.
3. Restart the computer after the software updater finishes.

Open the terminal and fix broken dependencies:
```sh
sudo apt-get install -f
```
If the dpkg manager is locked, reboot the machine and install the updates.

### Oracle Virtual Box Communication
Perform these steps on each guest machine (ubuntu1, ubuntu2, and ubuntu3):
1. Check the internet connection by pinging google.com: `ping google.com`
2. Check or correct the hostname by opening the hostname file: `sudo nano /etc/hostname`
3. Adjust the hostname to the name of the machine (e.g., ubuntu2) and save the changes.

Refer to the provided screenshots for visual guidance throughout these steps.

![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3015-33-31.png "Manager")

![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3017-27-46.png "Betriebssystem")

![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3017-28-20.png "Größe")

![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3017-28-55.png "Platte")

![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3017-30-07.png "Typ")

![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3017-30-40.png "Storage")

![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3017-32-17.png "Result")

![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3017-34-37.png "Network")

![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3017-35-30.png "Name")

![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3017-37-28.png "iso")

![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3017-49-39.png "install")

![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3017-55-56.png "credentials")

![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3017-58-02.png "install")

![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-30%2018-23-48.png "clone")

![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3020-38-09.png "installed")

![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3020-50-41.png "NAT")

Set the Adapter 2 to *Host-only Adapter*.
![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3021-01-11.png "Host-only Adapter")

Repeat for *ubuntu1*, *ubuntu2*, *ubuntu3*: Start the virtual machine 
![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3018-15-59.png "Start Screen")

Skip the setup screens. Confirm to install the updates.
![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3122-26-53.png "Install updates")

The software updater finished, restart the computer. 
![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3122-36-07.png "Reboot")

Open the terminal and fix broken dependencies.
```sh
sudo apt-get install -f
```
Check the internet connection, ping google.
```sh
ping google.com
```
![Ping google](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-31%2022-46-40.png "Ping google")

Check or correct the hostname e.g. for *ubuntu2* change the hostname from *ubuntu1* to *ubuntu2* (cloned machine). Open the hostname file.
```sh
sudo nano /etc/hostname
```
![Open hostname](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3123-03-55.png "Openhostname")

Adjust the hostname to the name of the machine e.g. *ubuntu2*
![Adjust hostname](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3123-00-32.png "Adjust hostname")

Save the changes and close with *ctrl + x*, press *Y* and *enter*.
![Hostname changed](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3123-09-42.png "Hostname changed")


Install net-tools:
```sh
sudo apt install net-tools
```

net-tools
![net-tools image](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3122-52-20.png "net-tools")

Determine the second Ethernet name.
```sh
$ ifconfig -a 
```
![ifconfig output](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3122-57-22.png "Ping google")

The second Ethernet name here is *enp0s8*. 

Edit the interfaces file.
```sh
sudo nano etc/network/interfaces
```
Append the following lines:
```
auto <second ethernet name>
iface <second ethernet name> inet static
address <static ip>
netmask 255.255.255.0
```

For example, using *enp0s8* as the second Ethernet name:
- *ubuntu1*
```
auto enp0s8
iface enp0s8 inet static
address 192.168.56.101
netmask 255.255.255.0
```

- *ubuntu2*
```
auto enp0s8
iface enp0s8 inet static
address 192.168.56.102
netmask 255.255.255.0
```

- *ubuntu3*
```
auto enp0s8
iface enp0s8 inet static
address 192.168.56.103
netmask 255.255.255.0
```

Remember to adjust the Ethernet name if needed.

Open a terminal on the host of the virtual machine (VM) and determine the host's hostname.
```sh
hostname -f
```
The hostname in this example is angel-primergy.

Open a terminal on the *ubuntu1* machine. Edit the hosts file.
```sh
sudo nano etc/hosts
```

Update the hostname on the second line for each machine:
- *ubuntu1*
```
127.0.1.1 ubuntu1
```

- *ubuntu2*
```
127.0.1.1 ubuntu2
```

- *ubuntu3*
```
127.0.1.1 ubuntu3
```

Add the IP addresses and hostnames at the end of the file.
For *ubuntu1*:
```
192.168.56.1 angel-primergy
192.168.56.102	ubuntu2
192.168.56.103	ubuntu3
```

For *ubuntu2*:
```
192.168.56.1 angel-primergy
192.168.56.101	ubuntu1
192.168.56.103	ubuntu3
```

For *ubuntu3*:
```
192.168.56.1 angel-primergy
192.168.56.101	ubuntu1
192.168.56.102	ubuntu2
```

Save and close the file using Ctrl+X, Y, and Enter.

Restart the machines.
```sh
sudo reboot
```

Verify the changes to the static IP addresses on *ubuntu1*, *ubuntu2*, and *ubuntu3*.
```sh
ifconfig -a
```

Ensure the correct hostname is set on each machine as *ubuntu1*, *ubuntu2*, or *ubuntu3*.
```sh
hostname -f
```

Open a terminal on the host machine and edit the hosts file.
```sh
sudo nano etc/hosts
```

Add the following lines:
```
192.168.56

Install the three essential components of Kubernetes. Kubelet is the base component responsible for managing processes on individual machines. Kubeadm is used for Kubernetes cluster administration, while kubectl manages configurations on various nodes within the cluster.
```sh
apt-get install -y kubelet kubeadm kubectl 
```
Modify the Kubernetes configuration file.
```sh
nano /etc/systemd/system/kubelet.service.d/10-kubeadm.conf
```
In a text editor, add the following line after the last "Environment Variable":
```sh
Environment=”cgroup-driver=systemd/cgroup-driver=cgroupfs”
```
Press Ctrl+X, Y, and Enter to save. Perform these steps only on the master node (kmaster VM).
Enable the Docker service.
```sh
systemctl enable docker.service
```

Start the Kubernetes cluster from the master machine.
```sh
# kubeadm init --apiserver-advertise-address=<ip-address-of-kmaster-vm> --pod-network-cidr=192.168.0.0/16
```
Follow the output instructions. Execute commands (1) as a non-root user to enable kubectl CLI usage. Save command (2) for future use to join nodes to the cluster.
Run the commands from the output as a non-root user.
```sh
$ mkdir -p $HOME/.kube
$ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
$ sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
Verify all pods are running except kube-dns.
```sh
kubectl get pods -o wide --all-namespaces
```
To resolve the 'kube-dns' issue, install the CALICO pod network.
```sh
kubectl apply -f https://docs.projectcalico.org/v3.0/getting-started/kubernetes/installation/hosted/kubeadm/1.7/calico.yaml 
```
Check that all pods have shifted to the running state.
Install the dashboard.
```sh
kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/recommended/kubernetes-dashboard.yaml
```
By default, the dashboard will not be visible on the Master VM. Change this setting.
```sh
kubectl proxy
```
Access the dashboard in the browser using the Master VM's browser: http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/.
Enter credentials, select Token, and proceed to get the token.
Create a dashboard service account and obtain its credentials.
Run these commands in a new terminal or the kubectl proxy command will stop.
Create a dashboard service account in the default namespace.
```sh
kubectl create serviceaccount dashboard -n default
```
Add cluster binding rules to the dashboard account.
```sh
kubectl create clusterrolebinding dashboard-admin -n default \
  --clusterrole=cluster-admin \
  --serviceaccount=default:dashboard
```
Obtain the token required for dashboard login.
```sh
kubectl get secret $(kubectl get serviceaccount dashboard -o jsonpath="{.secrets[0].name}") -o jsonpath="{.data.token}" | base64 --decode
```
Paste this token into the Dashboard Login Page where you selected the Token option.
You have successfully logged into the dashboard!

For the Kubernetes Node VM (knode) only:
Join the node to the cluster. This is likely the only step to perform on the node after installing Kubernetes.
Run the join command saved during the 'kubeadm init' command on the master with "sudo".
```sh
sudo kubeadm join --apiserver-advertise-address=<ip-address-of-the master> --pod

### Data Science Tools

#### Installing R Server
Begin by setting up R for your project. To install R Server, visit the [R Server download page](https://www.rstudio.com/products/rstudio/download-server/).

To get started with R Server, refer to the [Getting Started guide](https://support.rstudio.com/hc/en-us/articles/200552306-Getting-Started).

Access the R Server through http://<server-ip>:8787.

Refer to the [Server Management documentation](http://docs.rstudio.com/ide/server-pro/index.html) for details on stopping and starting the dashboard.

Add a user to the [rstudio-admin usergroup](https://www.howtogeek.com/50787/add-a-user-to-a-group-or-second-group-on-linux/) by first creating the group:
```sh
$ sudo groupadd rstudio-admins
```
Then, add the user to the group:
```sh
$ sudo usermod -a -G rstudio-admins <username>
```
Note that the open-source version does not include user dashboard privileges and security features.

#### Installing Python pyspark
To install Python pyspark, follow the instructions in the [pyspark documentation](https://pypi.org/project/pyspark).

#### Installing Microsoft Machine Learning Server
To use Microsoft Machine Learning Server, set it up on a local Linux server. This will enable you to access Microsoft ML libraries using Python or Spark. Refer to the [Microsoft machine learning server documentation](https://docs.microsoft.com/en-us/machine-learning-server/) for more information.

#### Installing TensorFlow
To install TensorFlow, visit the [TensorFlow website](www.tensorflow.com).

#### Forecasting Techniques
When selecting a forecasting algorithm, consider options from R or Python libraries and pay close attention to the details. You may also try Coffe, TensorFlow on Spark, or GP. Visualize the results for better understanding. To access more data, connect to a full Hadoop data lake using, for example, HBase, or utilize the Cassandra Connector.
