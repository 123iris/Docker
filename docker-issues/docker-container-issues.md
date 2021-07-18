# Docker container package issues

## 1. E: unable to locate package vim on nginx docker container


#### Solution:

* The solution is followed from this [source](https://unix.stackexchange.com/questions/336392/e-unable-to-locate-package-vim-on-debian-jessie-simplified-docker-container)

```
 1. apt-get update
 2. apt-get install apt-file
 3. apt-file update
 4. apt-get install vim     # now finally this will work !!!
 
 	(or)
 apt-get update && apt-get install apt-file -y && apt-file update && apt-get install vim -y	
```
now you can use vim

## Install systemctl on ubuntu docker container

#### Solution:

* The solution is followed from this [source](https://www.edureka.co/community/63212/how-can-i-install-systemctl-tool-in-docker-container#:~:text=In%20docker%20container%20if%20you,systemctl%20using%20the%20below%20command.)
 
``` 
syed@syed-Latitude-3490:~$ docker ps -a
CONTAINER ID   IMAGE      COMMAND                  CREATED              STATUS              PORTS                               NAMES
093dfb0ea238   centos:7   "/bin/bash"              About a minute ago   Up About a minute                                       naughty_meninsky
cc5f91f4020d   nginx      "/docker-entrypoint.…"   15 minutes ago       Up 6 minutes        0.0.0.0:80->80/tcp, :::80->80/tcp   agitated_wilson
syed@syed-Latitude-3490:~$ docker exec -it 093dfb0ea238 /bin/bash
[root@093dfb0ea238 /]# yum install /usr/bin/systemctl
```

## Not able to start nginx docker container to port 80:80 because port 0.0.0.0:80 is busy

#### Solution:

* The solution is followed from this [source](https://www.digitalocean.com/community/questions/nginx-not-starting-address-already-in-use-nginx-bind-to-0-0-0-0-80-failed)

* check whether nginx is running on host machine or not
* If not try to start

```
syed@syed-Latitude-3490:~$ sudo systemctl start  nginx
Job for nginx.service failed because the control process exited with error code.
See "systemctl status nginx.service" and "journalctl -xe" for details.
```
* check the status of nginx serivce

```
syed@syed-Latitude-3490:~$ systemctl status nginx.service
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/lib/systemd/system/nginx.service; disabled; vendor preset: enabled)
     Active: failed (Result: exit-code) since Fri 2021-05-28 18:33:08 IST; 47s ago
       Docs: man:nginx(8)
    Process: 65223 ExecStartPre=/usr/sbin/nginx -t -q -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
    Process: 65224 ExecStart=/usr/sbin/nginx -g daemon on; master_process on; (code=exited, status=1/FAILURE)

May 28 18:33:07 syed-Latitude-3490 nginx[65224]: nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)
May 28 18:33:07 syed-Latitude-3490 nginx[65224]: nginx: [emerg] bind() to [::]:80 failed (98: Address already in use)
May 28 18:33:07 syed-Latitude-3490 nginx[65224]: nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)
May 28 18:33:07 syed-Latitude-3490 nginx[65224]: nginx: [emerg] bind() to [::]:80 failed (98: Address already in use)
May 28 18:33:08 syed-Latitude-3490 nginx[65224]: nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)
May 28 18:33:08 syed-Latitude-3490 nginx[65224]: nginx: [emerg] bind() to [::]:80 failed (98: Address already in use)
May 28 18:33:08 syed-Latitude-3490 nginx[65224]: nginx: [emerg] still could not bind()
May 28 18:33:08 syed-Latitude-3490 systemd[1]: nginx.service: Control process exited, code=exited, status=1/FAILURE
May 28 18:33:08 syed-Latitude-3490 systemd[1]: nginx.service: Failed with result 'exit-code'.
May 28 18:33:08 syed-Latitude-3490 systemd[1]: Failed to start A high performance web server and a reverse proxy server.
```


* Check which service is listening to port 80.

```
The service which is already listening on port 80 might be Apache or any other web server listening on port 80.

To check that, you could run the following command:

syed@syed-Latitude-3490:~$ sudo netstat -plant | grep 80
```

* Now we conclude that apache is using port 80 so we stop that service

```
syed@syed-Latitude-3490:~$ sudo systemctl stop apache2
```

* wow, now the nginx docker service is running.

```
syed@syed-Latitude-3490:~$ docker run -itd -p 80:80 nginx 
e73a85934687580e486c38cfb39c5e4bfff365712ace3f7141d02f27a8a48c3a

syed@syed-Latitude-3490:~$ docker ps 
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS         PORTS                               NAMES
e73a85934687   nginx     "/docker-entrypoint.…"   10 seconds ago   Up 9 seconds   0.0.0.0:80->80/tcp, :::80->80/tcp   nervous_morse
syed@syed-Latitude-3490:~$ 

```