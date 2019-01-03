# K8s Spark
## Downloads
- [Oracle Virtual Box](https://www.virtualbox.org/wiki/Downloads)
- [Ubuntu 18.10](http://releases.ubuntu.com/)

## Oracle virtual machines

#### Oracle virtual Box Ubuntu
Install Oracle virtual Box on your host system. 

Create a new virtual machine

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

Choose your keyboard layout, select 
- *normal installation*
- *download updates while installing Ubuntu*
- *erase disk and install ubuntu*

Choose your time zone and set the credentials with the username *ubuntu* and the password *ubuntu*. 

Shut down the VM by closing the window, don't save and eject the installer iso from the VM's CD drive. 

Clone the VM *ubuntu1* two times and name the clones *ubuntu2* and *ubuntu3*,

![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-30%2018-23-48.png "clone")

The result
![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3020-38-09.png "installed")

On every virtual machine check the network setting, provide NAT adapter and host-only adapter,
![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3020-50-41.png "NAT")

Set the Adapter 2 to *Host-only Adapter*.
![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3021-01-11.png "Host-only Adapter")

Do the next steps of this part for *ubuntu1*, *ubuntu2*, *ubuntu3*.
Start the virtual machine 
![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3018-15-59.png "Start Screen")

Skip the setup screens. Confirm to install the updates.
![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3122-26-53.png "Install updates")

The software updater finished. Restart the computer. 
![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3122-36-07.png "Reboot")

Open the terminal and fix broken dependencies.
```sh
sudo apt-get install -f
```

If the dpkg manager is locked, reboot the machine and install the updates. Check e.g. by fixing broken dependencies as above.

##### Oracle virtual Box Communication
Perform this steps on every guest machine *ubuntu1*, *ubuntu2* and *ubuntu3*.

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

Install net-tools.
```sh
sudo apt install net-tools
```
![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3122-52-20.png "Ping google")

Check the second ethernet name.
```sh
$ ifconfig -a 
```
![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3122-57-22.png "Ping google")

The <second ethernet name> here is *enp0s8*. 

Open interfaces file.
```sh
sudo nano etc/network/interfaces
```
Add to the bottom 
>auto <second ethernet name>
>iface <second ethernet name> inet static
>address <static ip>
>netmask 255.255.255.0

E.g. with all <second ethernet name> enp0s8 this is
for *ubuntu1*
>auto enp0s8
>iface enp0s8 inet static
>address 192.168.56.101
>netmask 255.255.255.0

for *ubuntu2*
>auto enp0s8
>iface enp0s8 inet static
>address 192.168.56.102
>netmask 255.255.255.0

for *ubuntu3*
>auto enp0s8
>iface enp0s8 inet static
>address 192.168.56.103
>netmask 255.255.255.0

You might have to adjust the ethernet name accordingly.

Open a terminal on the host of the vm and find out the
<host of vm hostname>.
```sh
hostname -f
```
The hostname here is angel-primergy.

Open a terminal on the machine *ubuntu1*. Open the hosts file.
```sh
sudo nano etc/hosts
```

On the second line adjust the hostname for *ubuntu1*
>127.0.1.1 ubuntu1

On the second line adjust the hostname for *ubuntu2*
>127.0.1.1 ubuntu2

On the second line adjust the hostname for *ubuntu23
>127.0.1.1 ubuntu3

Add the ip addresses and hostnames to the bottom of the file.
For *ubuntu1*
>192.168.56.1 angel-primergy
>192.168.56.102	ubuntu2
>192.168.56.103	ubuntu3

For *ubuntu2*
>192.168.56.1 angel-primergy
>192.168.56.101	ubuntu1
>192.168.56.103	ubuntu3

For *ubuntu3*
>192.168.56.1 angel-primergy
>192.168.56.101	ubuntu1
>192.168.56.102	ubuntu2

Save and close the file with *strg + x* and *Y* and *enter*.

Reboot the machines.
```sh
sudo reboot
```

Run ifconfig -a to check the changes to the static ip adress on *ubuntu1*, *ubuntu2* and *ubuntu3*
```sh
ifconfig -a
```

Check the host name is set correct now on every machine to *ubuntu1*, *ubuntu2* or *ubuntu3*. 
```sh
hostname -f
```

Open a terminal on the host machine *<name of host>*. Open the hosts file.
```sh
sudo nano etc/hosts
```

Add at the bottom
>192.168.56.101 ubuntu1
>192.168.56.102	ubuntu2
>192.168.56.103	ubuntu3

Make sure to perform the steps on every machine.

##### Checkpoint: Oracle virtual Box Communication
Ping the host machine and every guest machine from the host machine and from every guest machine
```sh
ping angel-primergy
ping ubuntu1
ping ubuntu2
ping ubuntu3
```

##### Oracle virtual Box SSH
On *ubuntu1*, *ubuntu2* and *ubuntu3*: Install *ssh* and *openserver-ssh* 
```sh
sudo apt-get install openssh-server
```
On *ubuntu1*, *ubuntu2* and *ubuntu3*: Restart the network with linux command or restart all clients
Type the following command:
```sh
$ sudo /etc/init.d/ssh restart
```
or
```sh
$ sudo service ssh restart
```

Switch to the host sytem and open a terminal.
Remove the vms from the known hosts
```sh
ssh-keygen -R ubuntu1
ssh-keygen -R ubuntu2
ssh-keygen -R ubuntu3
```

ssh into the machines from the host machine
```sh
ssh ubuntu@ubuntu1
```
Leave the passphrase empty. Enter the user password *ubuntu*. You are now logged in. Exit the machine, type
```sh
exit
```
Also ssh into the machine ubuntu2 from the host machine 
```sh
ssh ubuntu@ubuntu2
```
Leave the passphrase empty. Enter the user password *ubuntu*. You are now logged in. Exit the machine, type
```sh
exit
```

Also ssh into the machine ubuntu3 from the host machine
```sh
ssh ubuntu@ubuntu3
```
Leave the passphrase empty. Enter the user password *ubuntu*. You are now logged in. Exit the machine, type
```sh
exit
```

On *ubuntu1*, *ubuntu2* and *ubuntu3*: SSH into both guest machines and into the host machine
```sh
ssh ubuntu@<name of guest machine>
```
```sh
ssh <user>@<name of host machine>
```
e.g.
```sh
ssh angel@angel-primergy
```

Shutdown all machines and create a Snapshot for each machine. Power the machines back up.

Perform similiar steps for more machines. 

##### Oracle virtual Box passwordless SSH

For *ubuntu1*, *ubuntu2*, *ubuntu3* and *angel-primergy* start a terminal, type
```sh
ssh-keygen  
```
Pess enter to save as default name. Press enter to skip to create a password.

For each guest host and remote host clear known hosts
```sh
ssh-keygen -R ubuntu1
ssh-keygen -R ubuntu2
ssh-keygen -R ubuntu3
ssh-keygen -R angel-primergy
```
Copy content of ida_rsa.pub to ~/.ssh/authorized_keys
```sh
ssh-copy-id ubuntu@ubuntu1
ssh-copy-id ubuntu@ubuntu2
ssh-copy-id ubuntu@ubuntu3
ssh-copy-id angel@angel-primergy
```
#### Checkpoint: Oracle virtual Box passwordless SSH
SSH into all machines. Start terminal on the host machine, then
```sh
ssh angel@angel-primergy
ssh ubuntu@ubuntu1
exit
ssh ubuntu@ubuntu2
exit
ssh ubuntu@ubuntu3
ssh ubuntu@ubuntu3
ssh angel@angel-primergy
exit
ssh ubuntu@ubuntu1
exit
ssh ubuntu@ubuntu2
ssh ubuntu@ubuntu2
ssh angel@angel-primergy
exit
ssh ubuntu@ubuntu3
exit
ssh ubuntu@ubuntu1
ssh ubuntu@ubuntu1
ssh angel@angel-primergy
exit
ssh ubuntu@ubuntu2
exit
ssh ubuntu@ubuntu3
```
Close terminal

#### Create a snapshot
Shutdown all machines and create a Snapshot for each machine. Power the machines back up.

## Install Kubernetes
#### Method I: Install Kubernetes
Pre-requisites To Install Kubernetes 

The following setting is recommended

Master:

    2 GB RAM
    2 Cores of CPU

Slave/ Node:

    1 GB RAM
    1 Core of CPU

The machines can be set up as stated earlier.

Let’s call the the master as ‘kmaster‘ and node as ‘knode‘. 

Login as ‘sudo’ user 
```sh
$ sudo su
# apt-get update
```

Turn off swap space. Open the ‘fstab’ file and comment out the line which has mention of swap partition.
```sh
swapoff -a
nano /etc/fstab
```
Then press ‘Ctrl+X’, then press ‘Y’ and then press ‘Enter’ to Save the file.

Update The Hostnames. Rename the master machine to ‘kmaster’ and your node machine to ‘knode’. 
```sh
nano /etc/hostname
```
Then press ‘Ctrl+X’, then press ‘Y’ and then press ‘Enter’ to Save the file.
Run the following command on both machines to note the IP addresses of each.
```sh
ifconfig
```
Now go to the ‘hosts’ file on both the master and node and add an entry specifying their respective IP addresses along with their names ‘kmaster’ and ‘knode’. 

Also check that the ip adresses are set to static as stated earlier. 
```sh
nano /etc/network/interfaces
```
After this, restart your machine(s).

Make sure openssh-server is installed as stated earlier. 
Install Docker because Docker images will be used for managing the containers.
```sh
sudo su
apt-get update 
apt-get install -y docker.io
```

Install these 3 essential components for setting up Kubernetes environment: kubeadm, kubectl, and kubelet.
```sh
# apt-get update && apt-get install -y apt-transport-https curl
# curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
# cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF
# apt-get update
```

Install the 3 essential components. Kubelet is the lowest level component in Kubernetes. It’s responsible for what’s running on an individual machine. Kuebadm is used for administrating the Kubernetes cluster. Kubectl is used for controlling the configurations on various nodes inside the cluster.
```sh
apt-get install -y kubelet kubeadm kubectl 
```
Change the configuration file of Kubernetes.
```sh
nano /etc/systemd/system/kubelet.service.d/10-kubeadm.conf
```
 open a text editor, enter the following line after the last “Environment Variable”:
```sh
Environment=”cgroup-driver=systemd/cgroup-driver=cgroupfs”
```
Now press Ctrl+X, then press Y, and then press Enter to Save.
These steps will only be executed on the master node (kmaster VM).
We will now start our Kubernetes cluster from the master’s machine.
```sh
# kubeadm init --apiserver-advertise-address=<ip-address-of-kmaster-vm> --pod-network-cidr=192.168.0.0/16
```
You will get the below output. The commands marked as (1), execute them as a non-root user. This will enable you to use kubectl from the CLI
The command marked as (2) should also be saved for future. This will be used to join nodes to your cluster.
Run the commands from the above output as a non-root user.
```sh
$ mkdir -p $HOME/.kube
$ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
$ sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
Verify and see all pods running expect kube-dns
```sh
kubectl get pods -o wide --all-namespaces
```
all the pods are running except one: ‘kube-dns’. For resolving this we will install a pod network. To install the CALICO pod network, run the following command
```sh
kubectl apply -f https://docs.projectcalico.org/v3.0/getting-started/kubernetes/installation/hosted/kubeadm/1.7/calico.yaml 
```
See all pods shift to the running state.
Install the dashboard. 
```sh
kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/recommended/kubernetes-dashboard.yaml
```
If you check the pods again, your dashboard is in running state. 
By default dashboard will not be visible on the Master VM, change this.
```sh
kubectl proxy
```
You get an output like starting to serve at 127.0.0.1:8001.
View the dashboard in the browser, navigate to the following address in the browser of your Master VM: http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/.
Enter credentials, choose Token and work on getting the token next.
Create the service account for the dashboard and get it’s credentials.
Run all these commands in a new terminal, or your kubectl proxy command will stop. 
Create a service account for dashboard in the default namespace.
```sh
kubectl create serviceaccount dashboard -n default
```
Add the cluster binding rules to your dashboard account
```sh
kubectl create clusterrolebinding dashboard-admin -n default \
  --clusterrole=cluster-admin \
  --serviceaccount=default:dashboard
```
The token required for your dashboard login
```sh
kubectl get secret $(kubectl get serviceaccount dashboard -o jsonpath="{.secrets[0].name}") -o jsonpath="{.data.token}" | base64 --decode
```
Copy this token and paste it in Dashboard Login Page where you chose the Token option.
You have successfully logged into your dashboard!

For Only Kubernetes Node VM (knode): 
It is time to get your node, to join the cluster! This is probably the only step that you will be doing on the node, after installing kubernetes on it.
Run the join command that you saved, when you ran ‘kubeadm init’ command on the master.
Run this command with “sudo”. 
```sh
sudo kubeadm join --apiserver-advertise-address=<ip-address-of-the master> --pod-network-cidr=192.168.0.0/16
```
The Kuberenetes Cluster is ready. Similiarly add more nodes. 

#### Method II: Install Kubernetes with conjure-u
Install lxd 
```sh
snap install lxd
```

Set up lxd with default values
```sh
snap install lxd
```

Install conjure-up
```sh
sudo snap install conjure-up --classic
```

Add users tp usergroup lxd
```sh
sudo usermod -a -G lxd <user>
newgrp lxd
```
Check storage
```sh
/snap/bin/lxc storage show default
```
```sh
config:
  source: /var/snap/lxd/common/lxd/storage-pools/default
description: ""
name: default
driver: dir
used_by:
- /1.0/profiles/default
```
Check network
```sh
/snap/bin/lxc network show lxdbr0
```
```sh
config:
  ipv4.address: 10.101.64.1/24
  ipv4.nat: "true"
  ipv6.address: none
  ipv6.nat: "false"
description: ""
name: lxdbr0
type: bridge
used_by: []
managed: true
```

You will also want to make sure that the LXD default profile is set to use lxdbr0 as its bridge:
```sh
/snap/bin/lxc profile show default
```
```sh
config: {}
description: Default LXD profile
devices:
  eth0:
    nictype: bridged
    parent: lxdbr0
    type: nic
  root:
    path: /
    pool: default
    type: disk
name: default
used_by: []
```

Check internet access
```sh
lxc launch ubuntu:16.04 u1
lxc exec u1 ping ubuntu.com
```

Restart the machine
```sh 
sudo init 6
sudo lxd init
```
For the settings, leave all as default expect
Name of storage backend to use [...]: dir
What Ipv6 adress should be used[...]: none

```sh
lxc list
lxc launch ubuntu:16.04
sudo snap install conjure-up --classic
conjure-up kubernetes
```
Follow the installation steps. Choose Kubernetes full installation, choose local installation and leave all other to default. 

Sidenote: Conjure-up has an option for a GPU accelerated setup for a Hadoop Cluster (not tested here).

### Install Kubernetes Spark
Follow the official Kubernetes Spark example documentation
[Kubernetes Spark Git](https://github.com/kubernetes/examples/tree/master/staging/spark)

### Read and write data from cassandra
This is not a [five minute tutorial](https://github.com/datastax/spark-cassandra-connector/blob/master/doc/0_quick_start.md)

### Data Science Tools

##### Install R Server
Start with R for this purpose.
Install (R Server)[https://www.rstudio.com/products/rstudio/download-server/]

Get [started with R Server](https://support.rstudio.com/hc/en-us/articles/200552306-Getting-Started)

Access R Server over http://<server-ip>:8787. 

Check the [Server management](http://docs.rstudio.com/ide/server-pro/index.html) especially how to stop and start the dashboard.

Add a user to the [usergroup rstudio-admin](https://www.howtogeek.com/50787/add-a-user-to-a-group-or-second-group-on-linux/)

This is basically create group
```sh
$ sudo groupadd rstudio-admins
```
and add user to group
```sh
$ sudo usermod -a -G rstudio-admins <username>
```
The open source version has no user dashboard privileges and no security functionalities. 

##### Install Python pyspark
Follow the [pyspark documentation](https://pypi.org/project/pyspark)

##### Install Microsoft Machine Learning Server
Use [Microsoft machine learning server](https://docs.microsoft.com/en-us/machine-learning-server/) on a linux local server to access microsoft ML libraries with Python or Spark.

##### Install Tensorflow 
 [Tensorflow](www.tensorflow.com)
 
##### Forecasting
For forecasting, choose the algorithm from the R or Python libraries and pay attention to the details. Also try Coffe, Tensforflow on Spark, GP. Visualize the results. 
To access more data, connect to a full hadoop data lake with e.g. HBase or use the Cassandra Connector. 



