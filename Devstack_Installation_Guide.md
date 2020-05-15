### Devstack installion guide on top of Virtual Machine.

##### VM Requirements: Minimum of 2CPUS and 4GB of RAM.
Here, I am using UBUNTU 18.04 Server image for VM.

IP : 192.168.11.21

Links:
https://docs.openstack.org/devstack/latest/


Devstack Installation Steps :-

```
ubuntu@ubuntu18:~$ sudo apt update -y

ubuntu@ubuntu18:~$ apt-get install software-properties-common -y
```

```

ubuntu@ubuntu18:~$ python -V
Python 2.7.17
ubuntu@ubuntu18:~$ python3 -V
Python 3.6.9

ubuntu@ubuntu18:~$ apt-get install -y python-systemd

````

```
root@ubuntu18:~# useradd -s /bin/bash -d /opt/stack -m stack

root@ubuntu18:~# echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/stack

root@ubuntu18:~# su - stack
```

```
stack@ubuntu18:~$ git clone https://github.com/openstack-dev/devstack.git -b stable/pike devstack/

stack@ubuntu18:~$ cd devstack/

stack@ubuntu18:~/devstack$ cat local.conf
[[local|localrc]]
ADMIN_PASSWORD=secret
DATABASE_PASSWORD=$ADMIN_PASSWORD
RABBIT_PASSWORD=$ADMIN_PASSWORD
SERVICE_PASSWORD=$ADMIN_PASSWORD
HOST_IP=192.168.11.21
RECLONE=yes
stack@ubuntu18:~/devstack$
```

```
stack@ubuntu18:~/devstack$ FORCE=yes ./stack.sh
```
