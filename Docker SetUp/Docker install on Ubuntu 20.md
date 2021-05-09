# Steps to Install Docker on Ubuntu VM 

The Instruction is followed from [here](https://phoenixnap.com/kb/install-docker-on-ubuntu-20-04)

### Step 1: Updating the Software Repository

* Start by opening a terminal window and updating the local repository:

&nbsp; In a terminal window, type: 
   
```
syed@k8sMaster:~$ sudo apt update -y

Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
....
```
Wait for the process to complete.


### Step 2: Downloading Dependencies

* Allow your Ubuntu 20.04 system to access the Docker repositories over HTTPS by running:

```
syed@k8sMaster:~$ sudo apt-get install apt-transport-https         \
                                             ca-certificates curl        \
                                             software-properties-common  \
                                             -y
```

* The above-mentioned command:
    * Gives the package manager permission to transfer files and data over https.
    * Allows the system to check security certificates.
    * Installs curl, a a tool for transferring data.
    * Adds scripts for managing software.
    
### Step 3: Adding Docker’s GPG Key

* Next, add the GPG key to ensure the authenticity of the software package:

```
syed@k8sMaster:~$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

OK
```

### Step 4: Installing the Docker Repository

* Now install the Docker repository using the command:

```
syed@k8sMaster:~$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu  $(lsb_release -cs)  stable"

```
The command installs the latest repository for your specific Ubuntu release (in this case, 20.04 Focal Fossa).

### Step 5: Installing the Latest Docker

* Start by updating the repository again:

```
syed@k8sMaster:~$ sudo apt update -y
```

* Now you can install the latest Docker version with:

```
syed@k8sMaster:~$ sudo apt-get install docker-ce -y
```

### Step 6: Verifying Docker Installation

* To confirm the installation check the version of Docker:

```
syed@k8sMaster:~$ docker --version
Docker version 20.10.6, build 370c289
```
* To see full specification of docker use the below command:

```
syed@k8sMaster:~$ docker version
Client: Docker Engine - Community
 Version:           20.10.6
 API version:       1.41
 Go version:        go1.13.15
 Git commit:        370c289
 Built:             Fri Apr  9 22:47:17 2021
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.24/version: dial unix /var/run/docker.sock: connect: permission denied
```

* From above command, you may notice that there is "permission denied" Error. 

* Make sure you have sudo access. Inorder to access full docker specification.

```
syed@k8sMaster:~$ sudo docker version
Client: Docker Engine - Community
 Version:           20.10.6
 API version:       1.41
 Go version:        go1.13.15
 Git commit:        370c289
 Built:             Fri Apr  9 22:47:17 2021
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          20.10.6
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.13.15
  Git commit:       8728dd2
  Built:            Fri Apr  9 22:45:28 2021
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.4.4
  GitCommit:        05f951a3781f4f2c1911b05e61c160e9c30eaa8e
 runc:
  Version:          1.0.0-rc93
  GitCommit:        12644e614e25b05da6fd08a38ffa0cfe1903fdec
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0

```

### Step 7: Start & Enable Docker Service 

* To start the Docker service run the following commands:

```
syed@k8sMaster:~$ sudo systemctl start docker
```

* Enable Docker to run at startup with:

```
syed@k8sMaster:~$ sudo systemctl enable docker

Synchronizing state of docker.service with SysV service script with /lib/systemd/systemd-sysv-install.
Executing: /lib/systemd/systemd-sysv-install enable docker
```

* To check the status of the service, use the command:

```
syed@k8sMaster:~$ sudo systemctl status docker

● docker.service - Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
     Active: active (running) since Sun 2021-05-09 11:16:48 IST; 6min ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 2884 (dockerd)
      Tasks: 12
     Memory: 42.2M
     CGroup: /system.slice/docker.service
             └─2884 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

May 09 11:16:46 k8sMaster dockerd[2884]: time="2021-05-09T11:16:46.873624227+05:30" level=warning msg="Your kernel does not support CPU realt>
May 09 11:16:46 k8sMaster dockerd[2884]: time="2021-05-09T11:16:46.874092614+05:30" level=warning msg="Your kernel does not support cgroup bl>
May 09 11:16:46 k8sMaster dockerd[2884]: time="2021-05-09T11:16:46.874527451+05:30" level=warning msg="Your kernel does not support cgroup bl>
May 09 11:16:46 k8sMaster dockerd[2884]: time="2021-05-09T11:16:46.875959381+05:30" level=info msg="Loading containers: start."
May 09 11:16:47 k8sMaster dockerd[2884]: time="2021-05-09T11:16:47.527712672+05:30" level=info msg="Default bridge (docker0) is assigned with>
May 09 11:16:47 k8sMaster dockerd[2884]: time="2021-05-09T11:16:47.968929860+05:30" level=info msg="Loading containers: done."
May 09 11:16:48 k8sMaster dockerd[2884]: time="2021-05-09T11:16:48.025708655+05:30" level=info msg="Docker daemon" commit=8728dd2 graphdriver>
May 09 11:16:48 k8sMaster dockerd[2884]: time="2021-05-09T11:16:48.026101824+05:30" level=info msg="Daemon has completed initialization"
May 09 11:16:48 k8sMaster systemd[1]: Started Docker Application Container Engine.
May 09 11:16:48 k8sMaster dockerd[2884]: time="2021-05-09T11:16:48.098442116+05:30" level=info msg="API listen on /run/docker.sock"
```

The output should show Docker is active (running).