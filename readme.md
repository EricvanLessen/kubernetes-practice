# Spark K8s 

#### Prerequisites
- [Oracle Virtual Box](https://www.virtualbox.org/wiki/Downloads)
- [Ubuntu 18.10](http://releases.ubuntu.com/)


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

Clone the VM *ubuntu1* two times and name the clones *ubuntu2* and *ubuntu3*.

![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-30%2018-23-48.png "clone")

The result
![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3020-38-09.png "installed")

On every virtual machine change the network setting, provide NAT adapter and host-only adapter
![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3020-50-41.png "NAT")

Set the Adapter 2 to *Host-only Adapter*
![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3021-01-11.png "Host-only Adapter")

Start the virtual machine *ubuntu1*
![alt text](https://raw.githubusercontent.com/EricTarantino/genericdemo/master/Bildschirmfotovom2018-12-3018-15-59.png "Start Screen")


Undergo the setup screens and press confirm to install the updates.

##### Oracle virtual Box Communication
Fix broken dependencies
```sh
sudo apt-get install -f
```
Install net-tools 
```sh
sudo apt install net-tools
```
Check the second ethernet name
```sh
$ ifconfig -a 
```

The name here is *enp0s8*. 

Open interfaces file.
```sh
sudo nano etc/network/interfaces
```
Add to the bottom
>auto enp0s8
>iface enp0s8 inet static
>address 192.168.56.100
>netmask 255.255.255.0

You might have to adjust the ethernet name accordingly.

Reboot
```sh
sudo reboot
```

Ping the host machine
```sh
ping 192.168.56.100
```

Open hosts file
```sh
sudo nano etc/hosts and add at the bottom
```
Add hostnames to the bottom
>192.168.56.101	ubuntu1
>192.168.56.102	ubuntu2
>192.168.56.103	ubuntu3

Install *ssh* and *openserver-ssh*
```sh
sudo apt-get install openssh-server
```
Restart the network with linux command or restart all clients
Type the following command:
```sh
$ sudo /etc/init.d/ssh restart
```
or
```sh
$ sudo service ssh restart
```
Repeat the steps for every virtual machine.

##### Oracle virtual Box passwordless SSH
Login to ubuntu@ubuntu1 and start a terminal, type
```sh
ssh-keygen  
```
Copy the public key to the target
```sh
scp ~/.ssh/id_rsa.pub ubuntu@ubuntu1
scp ~/.ssh/id_rsa.pub ubuntu@ubuntu2
scp ~/.ssh/id_rsa.pub ubuntu@ubuntu3
```

Connect to each remote host and copy content of ida_rsa.pub to ~/.ssh/authorized_keys
```sh
ssh ubuntu@ubuntu1
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
exit
```
```sh
ssh ubuntu@ubuntu2
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
exit
```
```sh
ssh ubuntu@ubuntu3
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
exit
```

#### current status

Remove known hosts, the all file (or explicit entries) ~/.ssh$ rm known_hosts. Known_hosts has the host information which is eventually corrupted. Login to all machines and confirm to add hosts to known hosts. → password is not needed any more. 

Repeat the same for ubuntu@ubuntu2 and for ubuntu@ubuntu3 (or login via ssh from ubuntu@ubuntu1) and repeat the same.

#### Kubernetes and Spark
Step 2 Production deployment with Kubernetes from Gitlab

Big Data Support with Kubernetes:
Set up Kubernetes:
https://lxd.readthedocs.io/en/latest/#installing-lxd-from-packages
https://conjure-up.io/
https://itnext.io/tutorial-part-1-kubernetes-up-and-running-on-lxc-lxd-b760c79cd53f
https://kubernetes.io/docs/getting-started-guides/ubuntu/
https://datasterix.com/2016/09/03/spark-cluster-using-multi-node-kubernetes-and-docker/
https://kubernetes.io/docs/tasks/tools/install-kubectl/

Docker Spark:
https://xuri.me/2016/03/27/running-apache-spark-on-yarn-with-docker.html
https://github.com/sequenceiq/docker-spark-native-yarn
https://hub.docker.com/r/semantive/spark

Kubernetes spark:
https://kubernetes.io/blog/2018/03/apache-spark-23-with-native-kubernetes/
https://medium.com/ymedialabs-innovation/apache-spark-on-a-multi-node-cluster-b75967c8cb2b
https://github.com/kubernetes/examples/tree/master/staging/spark
https://github.com/GoogleCloudPlatform/spark-on-k8s-operator/blob/master/docs/user-guide.md
https://spark.apache.org/docs/latest/running-on-kubernetes.html

To embedd the served architecture online in a node.js html page use a generic page like www.rpage-7435748.io and embedd with:
<iframe source=“https://www.rpage-7435748.io“>…
Then connect to challenge and to other webpages
→ Google Adds, Marketing, Facebook, SEO, node.js page
Check out how to build referer pages

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

##### Set up Tensorflow 

##### Set up GPU acceleration

##### Set up 

