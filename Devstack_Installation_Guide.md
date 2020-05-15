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

```
```
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

stack@ubuntu18:~/devstack$ openstack user list
+----------------------------------+-----------+
| ID                               | Name      |
+----------------------------------+-----------+
| 320931f6105b48b29f2a729497c0c63f | glance    |
| 333a527872d74fa38a5dd001cbeb2986 | nova      |
| 58fc011d7db64f87966bc0f4c8b70db7 | alt_demo  |
| 6024f468598f4e8eb790a3aba2c95d11 | admin     |
| ae9381811d6841c5af0cd6e6a3764c56 | neutron   |
| d6f12002d8404098a521ebfbcc07e4d7 | cinder    |
| ebe39e3913954abeacbb2aa387de06e8 | placement |
| f0251609a65345c29af2db05a63587bb | demo      |
+----------------------------------+-----------+

stack@ubuntu18:~/devstack$ openstack project list
+----------------------------------+--------------------+
| ID                               | Name               |
+----------------------------------+--------------------+
| 24e5e45118db4a028e33df7e349e1093 | service            |
| 4129b72c7ba6430a9ea417706dd22454 | admin              |
| 43ffe10c7ed645cdb29fc07e87f2cd69 | demo               |
| d023a4a0e60a4b789e6aafdfade2dd53 | invisible_to_admin |
| e875b8ef986c407c8a96d83fad080fa6 | alt_demo           |
+----------------------------------+--------------------+


stack@ubuntu18:~/devstack$ openstack image list
+--------------------------------------+--------------------------+--------+
| ID                                   | Name                     | Status |
+--------------------------------------+--------------------------+--------+
| a281f10e-4084-470f-863f-99d0dc4b7cbf | cirros-0.3.5-x86_64-disk | active |
+--------------------------------------+--------------------------+--------+

stack@ubuntu18:~/devstack$ openstack flavor list
+----+-----------+-------+------+-----------+-------+-----------+
| ID | Name      |   RAM | Disk | Ephemeral | VCPUs | Is Public |
+----+-----------+-------+------+-----------+-------+-----------+
| 1  | m1.tiny   |   512 |    1 |         0 |     1 | True      |
| 2  | m1.small  |  2048 |   20 |         0 |     1 | True      |
| 3  | m1.medium |  4096 |   40 |         0 |     2 | True      |
| 4  | m1.large  |  8192 |   80 |         0 |     4 | True      |
| 42 | m1.nano   |    64 |    0 |         0 |     1 | True      |
| 5  | m1.xlarge | 16384 |  160 |         0 |     8 | True      |
| 84 | m1.micro  |   128 |    0 |         0 |     1 | True      |
| c1 | cirros256 |   256 |    0 |         0 |     1 | True      |
| d1 | ds512M    |   512 |    5 |         0 |     1 | True      |
| d2 | ds1G      |  1024 |   10 |         0 |     1 | True      |
| d3 | ds2G      |  2048 |   10 |         0 |     2 | True      |
| d4 | ds4G      |  4096 |   20 |         0 |     4 | True      |
+----+-----------+-------+------+-----------+-------+-----------+


stack@ubuntu18:~/devstack$ openstack network list
+--------------------------------------+---------+----------------------------------------------------------------------------+
| ID                                   | Name    | Subnets                                                                    |
+--------------------------------------+---------+----------------------------------------------------------------------------+
| 51144887-89ee-404f-bbd9-7b57685cd740 | private | 776bd380-484f-45e8-b63d-ae78cb1832b3, fff2adc0-c74c-47af-82fa-7e1e49b44e68 |
| fd6dc423-0df8-479f-89dc-5d9ff471d559 | public  | 687d71d3-37db-4cca-92fa-a22a0ba15d8a, 7fa97df6-7c65-43a3-9c01-9820fef1c7dd |
+--------------------------------------+---------+----------------------------------------------------------------------------+


stack@ubuntu18:~/devstack$ openstack subnet list
+--------------------------------------+---------------------+--------------------------------------+---------------------+
| ID                                   | Name                | Network                              | Subnet              |
+--------------------------------------+---------------------+--------------------------------------+---------------------+
| 687d71d3-37db-4cca-92fa-a22a0ba15d8a | public-subnet       | fd6dc423-0df8-479f-89dc-5d9ff471d559 | 172.24.4.0/24       |
| 776bd380-484f-45e8-b63d-ae78cb1832b3 | ipv6-private-subnet | 51144887-89ee-404f-bbd9-7b57685cd740 | fd23:7d29:51d6::/64 |
| 7fa97df6-7c65-43a3-9c01-9820fef1c7dd | ipv6-public-subnet  | fd6dc423-0df8-479f-89dc-5d9ff471d559 | 2001:db8::/64       |
| fff2adc0-c74c-47af-82fa-7e1e49b44e68 | private-subnet      | 51144887-89ee-404f-bbd9-7b57685cd740 | 10.0.0.0/26         |
+--------------------------------------+---------------------+--------------------------------------+---------------------+
stack@ubuntu18:~/devstack$

```
