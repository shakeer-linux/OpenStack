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

#### After Installation

```
stack@ubuntu18:~/devstack$ source openrc
WARNING: setting legacy OS_TENANT_NAME to support cli tools.
stack@ubuntu18:~/devstack$ env | grep OS_
OS_USER_DOMAIN_ID=default
OS_AUTH_URL=http://192.168.11.21/identity
OS_PROJECT_DOMAIN_ID=default
OS_REGION_NAME=RegionOne
OS_PROJECT_NAME=demo
OS_IDENTITY_API_VERSION=3
OS_TENANT_NAME=demo
OS_AUTH_TYPE=password
OS_PASSWORD=secret
OS_USERNAME=demo
OS_VOLUME_API_VERSION=2
OS_CACERT=
stack@ubuntu18:~/devstack$
stack@ubuntu18:~/devstack$ OS_USERNAME=admin
stack@ubuntu18:~/devstack$ OS_PROJECT_NAME=admin
stack@ubuntu18:~/devstack$
stack@ubuntu18:~/devstack$ env | grep OS_
OS_USER_DOMAIN_ID=default
OS_AUTH_URL=http://192.168.11.21/identity
OS_PROJECT_DOMAIN_ID=default
OS_REGION_NAME=RegionOne
OS_PROJECT_NAME=admin
OS_IDENTITY_API_VERSION=3
OS_TENANT_NAME=admin
OS_AUTH_TYPE=password
OS_PASSWORD=secret
OS_USERNAME=admin
OS_VOLUME_API_VERSION=2
OS_CACERT=
stack@ubuntu18:~/devstack$

stack@ubuntu18:~/devstack$ openstack service list
+----------------------------------+-------------+----------------+
| ID                               | Name        | Type           |
+----------------------------------+-------------+----------------+
| 0d7d8c342aa04d15899187e20f407bf2 | cinderv3    | volumev3       |
| 0e4619245e9b4f499e001e2552deba84 | nova_legacy | compute_legacy |
| 4641b1402e274489bd75c2bcc898f34c | keystone    | identity       |
| 4fb822e4074647abb2ca8d72182f069e | nova        | compute        |
| 65b6728a4304476e989d79d8553116b3 | glance      | image          |
| abbffdad4d934d7ca94bfd09bb3ed3b0 | placement   | placement      |
| cd058fbb463d4dc4a552fa7227069ae9 | cinderv2    | volumev2       |
| d5188eb9e470488aa6cd2aeaa3fa41c7 | cinder      | volume         |
| e1a840a6920b49adac051fa3f447542d | neutron     | network        |
+----------------------------------+-------------+----------------+
stack@ubuntu18:~/devstack$

```
