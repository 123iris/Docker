# Installing Docker on Centos VM 

The Instruction is followed from [here](https://phoenixnap.com/kb/how-to-install-docker-centos-7)

#### Step 1: Update your System 

 In a terminal window, type: 

   
```
[mosipuser@k8Master1 ~]$ sudo yum update
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
....
```
Allow the operation to complete.


#### Step 2:  Install the Dependencies

 The next step is to download the dependencies required for installing Docker.<br >&nbsp; Type in the following command:

    
```
[mosipuser@k8Master1 ~]$ sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

#### Step 3:  Add the Docker Repository to CentOS

To install the edge or test versions of Docker, you need to add the Docker CE stable repository to your system.
To do so, run the command: 
 
 ```
 sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
 ```
 
#### Step 4: Install Docker On CentOS Using Yum
 
With everything set, you can finally move on to installing Docker on CentOS 7 by running:

```
[mosipuser@k8Master1 ~]$ sudo yum install docker -y
...
...
Complete
```

The system should begin the installation. Once it finishes, it will notify you the installation is complete and which version of Docker is now running on your system.

#### Step: 5 Manage Docker Service

Although you have installed Docker on CentOS, the service is still not running.<br>
To start the service, enable it to run at startup.<br>
Run the following commands in the order listed below.<br>
**Start Docker: **

 ```
 [mosipuser@k8Master1 ~]$ sudo systemctl start docker
 ```
 
 **Enable Docker: **
 
 ```
 [mosipuser@k8Master1 ~]$ sudo systemctl enable docker
 Created symlink from /etc/systemd/system/multi-user.target.wants/docker.service to /usr/lib/systemd/system/docker.service.
 ```
 
**Check the status of the service with: **
 
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
 
**To stop the Docker service : **
 
 ```
[mosipuser@k8Master1 ~]$ sudo systemctl stop docker
```


## Install a Specific Version of Docker on CentOS

 To install a specific version of Docker, start by listing the available releases.<br>
&nbsp; Type the following in your terminal window:.


```
yum list docker-ce --showduplicates | sort –r
```

The system should give you a list of different versions from the repositories you have enabled above.

**Install the selected Docker version with the command:**

```
sudo yum install docker-ce-<VERSION STRING>
```
