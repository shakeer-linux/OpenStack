### Flavor:
Flavor is used to what type of instance/vm to run or launch by the user. It has some parameters like RAM, VCPU’s, RootDisk, etc.


### VM / Instance Creation Flow in OpenStack:   
    
We can create instance in two ways generally.
1. CLI (with nova boot command )and 2. From Dashboard
2. When we requested for Instance first request
3. First the request of new instance converts in to REST API request and send it to **NOVA-API**
4. **NOVA-API** receive the request sends the request for validation auth-token and access permission to keystone.
5. **KEYSTONE** validates the token and sends the updated roles and permissions.
6. **NOVA-API** interacts with **NOVA-DATABASE** and creates initial db entry for new instance.
7. Then after **NOVA-API** send the request to **NOVA-SCHEDULER**.
8. **NOVA-SCHEDULER** intercuts with **NOVA-DATABASE** to find and appropriate host, returns appropriate host ID.
9. **NOVA-SCHEDULER** send the request to nova-compute for launching instance on appropriate host.
10. **NOVA-COMPUTE** picks the request and send the request to **NOVA-CONDUCTOR** to get the information such as flavor, host ID
11. **NOVA-CONDUCTOR** picks the request and interacts with **NOVA-DATABASE** returns the required information sends to **NOVA-COMPUTE**.
12. **NOVA-COMPUTE** pick the required information from **NOVA-CONDUCTOR**.
13. **NOVA-COMPUTE** does the REST call by passing auth token to **GLANCE-API** to get the Image from Glance to download.
14. **GLANCE-API** validates the token with keystone and **NOVA-COMPUTE** gets the image.
15. Again **NOVA-COMPUTE** does the REST-call by passing auth token to **NETWORK API** to get the IP address for instance.
16. **NEUTRON-SERVER** validates the auth token with keystone and **NOVA-COMPUTE** get the network information
17. Same Again **NOVA-COMPUTE** does the REST call by passing auth token to Volume API to attache the volumes to instance
18. **CINDER-API** Validates the token with keystone and **NOVA-COMPUTE** get the block storage information

19. **NOVA -COMPUTE** generates data for hypervisor driver and executes there request on Hypervisor to **LAUNCH INSTANCE**.


 
| Status | Task | Steps |
---|---|---|
| Build| scheduling | 1-9 |
| Build | networking | 15,16 |
| Build | block device mapping | 17,18 |
| Build | spawning | 19 |
|Active | None |

