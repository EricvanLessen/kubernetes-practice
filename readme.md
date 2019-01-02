# Oracle K8s Spark Cluster

#### Prerequisites
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


## Kubernetes and Spark

#### Install Kubernetes

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
lxc list
lxc launch ubuntu:16.04
sudo snap install conjure-up --classic
conjure-up kubernetes
```

Settings as [following]( https://www.youtube.com/watch?v=EkhjNLFaQAY)

Sources: [LXD](https://lxd.readthedocs.io/en/latest/#installing-lxd-from-packages), [conjure-up]( https://conjure-up.io/)

#### Install Spark on Kubernetes

[Kubernetes Spark Git](https://github.com/kubernetes/examples/tree/master/staging/spark)

[Kubernetes Spark Docs](https://kubernetes.io/blog/2018/03/apache-spark-23-with-native-kubernetes/)

## Data Science Tools

##### Install R Server
Start with R for this purpose.
Install R Server
https://www.rstudio.com/products/rstudio/download-server/
Control
https://support.rstudio.com/hc/en-us/articles/200552306-Getting-Started
Access:
$ http://<server-ip>:8787
Server management
http://docs.rstudio.com/ide/server-pro/index.html
Stop, start, dashboard:
http://docs.rstudio.com/ide/server-pro/server-management.html
Add user to the usergroup rstudio-admin
https://www.howtogeek.com/50787/add-a-user-to-a-group-or-second-group-on-linux/
Create group:
$ sudo groupadd rstudio-admins
Add user to group:
$ sudo usermod -a -G rstudio-admins angel
→ Open source version has no user dashboard privileges
Problem: RServer Comparison to RServer pro. The R Server solutions are costly
https://www.rstudio.com/products/rstudio-server-pro/

##### Install Python pyspark
https://pypi.org/project/pyspark
Spark is a fast and general cluster computing system for Big Data. It provides high-level APIs in Scala, Java, Python, and R, and an optimized engine that supports general computation graphs for data analysis. 

##### Install Microsoft Machine Learning Server
Solution 1: Use Microsoft machine learning server on a linux local server
https://docs.microsoft.com/en-us/machine-learning-server/ 

##### Install Tensorflow 


## Notes

#### On CentOS 
https://datasterix.com/2016/09/03/spark-cluster-using-multi-node-kubernetes-and-docker/
https://kubernetes.io/docs/tasks/tools/install-kubectl/
https://www.youtube.com/watch?v=zjmY0brIYvQ

#### Additional Notes
https://medium.com/ymedialabs-innovation/apache-spark-on-a-multi-node-cluster-b75967c8cb2b
https://github.com/GoogleCloudPlatform/spark-on-k8s-operator/blob/master/docs/user-guide.md
Set up Kubernetes
https://www.youtube.com/watch?v=6xJwQgDnMFE
Setup Kubernetes
https://www.youtube.com/watch?v=UWg3ORRRF60
Docker Spark:
https://xuri.me/2016/03/27/running-apache-spark-on-yarn-with-docker.html
https://github.com/sequenceiq/docker-spark-native-yarn
https://hub.docker.com/r/semantive/spark
Kubernetes spark:
https://kubernetes.io/blog/2018/03/apache-spark-23-with-native-kubernetes/
https://github.com/kubernetes/examples/tree/master/staging/spark
https://spark.apache.org/docs/latest/running-on-kubernetes.html
To embedd the served architecture online in a node.js html page use a generic page like www.rpage-7435748.io and embedd with:
<iframe source=“https://www.rpage-7435748.io“>…
Then connect to challenge and to other webpages
→ Google Adds, Marketing, Facebook, SEO, node.js page
Check out how to build referer pages



