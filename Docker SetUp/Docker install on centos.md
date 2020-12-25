# Steps to Install Docker on Centos VM 

The Instruction is followed from [here](https://phoenixnap.com/kb/how-to-install-docker-centos-7)

### Step 1: Update your System 

&nbsp; In a terminal window, type: 
   
```
[mosipuser@k8Master1 ~]$ sudo yum update
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
....
```
&nbsp; Allow the operation to complete.


### Step 2:  Install the Dependencies

&nbsp; The next step is to download the dependencies required for installing Docker.<br >&nbsp; Type in the following command:

    
```
[mosipuser@k8Master1 ~]$ sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

### Step 3:  Add the Docker Repository to CentOS

&nbsp; To install the edge or test versions of Docker, you need to add the Docker CE stable repository to your system.<br>
&nbsp; To do so, run the command: 
 
 ```
 sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
 ```
 
### Step 4: Install Docker On CentOS Using Yum
 
&nbsp; With everything set, you can finally move on to installing Docker on CentOS 7 by running:

```
[mosipuser@k8Master1 ~]$ sudo yum install docker -y
...
...
Complete
```

&nbsp; The system should begin the installation. Once it finishes, it will notify you the installation is complete and which version of Docker is now running on your system.

### Step: 5 Manage Docker Service

&nbsp; Although you have installed Docker on CentOS, the service is still not running.<br>
&nbsp; To start the service, enable it to run at startup.<br>
&nbsp; Run the following commands in the order listed below.<br>
&nbsp; **Start Docker :**

 ```
 [mosipuser@k8Master1 ~]$ sudo systemctl start docker
 ```
 
&nbsp; **Enable Docker :**
 
 ```
 [mosipuser@k8Master1 ~]$ sudo systemctl enable docker
 Created symlink from /etc/systemd/system/multi-user.target.wants/docker.service to /usr/lib/systemd/system/docker.service.
 ```
 
&nbsp; **Check the status of the service with :**
 
 ```
[mosipuser@k8Master1 ~]$ sudo systemctl status docker
● docker.service - Docker Application Container Engine
   Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; vendor preset: disabled)
   Active: active (running) since Fri 2020-12-11 18:46:19 IST; 47s ago
     Docs: http://docs.docker.com
 Main PID: 1677 (dockerd-current)
   CGroup: /system.slice/docker.service
           ├─1677 /usr/bin/dockerd-current --add-runtime docker-runc=/usr/libexec/docker/docker-runc-current --default-runtime=docker-runc ...
           └─1682 /usr/bin/docker-containerd-current -l unix:///var/run/docker/libcontainerd/docker-containerd.sock --metrics-interval=0 --...

Dec 11 18:46:17 k8Master1 dockerd-current[1677]: time="2020-12-11T18:46:17.629756028+05:30" level=info msg="libcontainerd: new conta...: 1682"
Dec 11 18:46:18 k8Master1 dockerd-current[1677]: time="2020-12-11T18:46:18.774220685+05:30" level=info msg="Graph migration to conte...econds"
...
...
Dec 11 18:46:19 k8Master1 dockerd-current[1677]: time="2020-12-11T18:46:19.161428550+05:30" level=info msg="API listen on /var/run/d...r.sock"
Hint: Some lines were ellipsized, use -l to show in full.
 ```
 
&nbsp; **To stop the Docker service :**
 
 ```
[mosipuser@k8Master1 ~]$ sudo systemctl stop docker
```

### Allow Non root user to work with Docker

* If any Non-Root user try to work with docker. It will display Permission denied Error.

```
[mosipuser@k8Worker0 ~]$ docker version
Client:
 Version:         1.13.1
 API version:     1.26
 Package version: 
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.26/version: dial unix /var/run/docker.sock: connect: permission denied
```
* To Avoid this Execute below command:

```
[mosipuser@k8Worker0 ~]$ sudo chmod 666 /var/run/docker.sock


# now Non-Root user are able to work with docker

[mosipuser@k8Worker0 ~]$ docker version
Client:
 Version:         1.13.1
 API version:     1.26
 Package version: docker-1.13.1-203.git0be3e21.el7.centos.x86_64
 Go version:      go1.10.3
 Git commit:      0be3e21/1.13.1
 Built:           Thu Nov 12 15:11:46 2020
 OS/Arch:         linux/amd64

Server:
 Version:         1.13.1
 API version:     1.26 (minimum version 1.12)
 Package version: docker-1.13.1-203.git0be3e21.el7.centos.x86_64
 Go version:      go1.10.3
 Git commit:      0be3e21/1.13.1
 Built:           Thu Nov 12 15:11:46 2020
 OS/Arch:         linux/amd64
 Experimental:    false

```

## Install a Specific Version of Docker on CentOS ( Not Recommended )

 <pre style="font-family:helvitica;font-size:15px">&nbsp; To install a specific version of Docker, start by listing the available releases.<br>
&nbsp; Type the following in your terminal window:.
</pre>

```
yum list docker-ce --showduplicates | sort –r
```

The system should give you a list of different versions from the repositories you have enabled above.

&nbsp; **Install the selected Docker version with the command :**

```
sudo yum install docker-ce-<VERSION STRING>
```


# References 

1. [Phoenixnap.com](https://phoenixnap.com/kb/huow-to-install-docker-centos-7)
2. [DigitalOcean](https://www.digitalocean.com/community/questions/how-to-fix-docker-got-permission-denied-while-trying-to-connect-to-the-docker-daemon-socket)