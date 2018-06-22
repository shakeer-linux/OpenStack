
### VM (or) Instance Creation Flow in OpenStack :   
    
We can create instance in two ways generally, one is from CLI (with nova boot command )and second is from Dashboard(with Launch Instance)

1. Dashboard or CLI gets the user credential and does the REST call to Keystone for authentication.
2. **KEYSTONE** authenticate the credentials and generate & send back auth-token which will be used for sending request to other Components through REST-call.
3. First the new instance **request**(*eigher CLI(with nova boot command) or DASHBOARD(with Launch Instance))* converts in to **REST API request** and send it to **NOVA-API**
4. **NOVA-API** receive the request sends the request for validation auth-token and access permission to **KEYSTONE**
5. **KEYSTONE** validates the token and sends the updated roles and permissions
6. **NOVA-API** interacts with **NOVA-DATABASE** and creates initial db entry for new instance.
7. Then after **NOVA-API** send the request to **NOVA-SCHEDULER**.
8. **NOVA-SCHEDULER** intercuts with **NOVA-DATABASE** to find and appropriate host, returns appropriate host ID.
9. **NOVA-SCHEDULER** send the request to **NOVA-COMPUTE** for launching instance on appropriate host.
10. **NOVA-COMPUTE** picks the request and send the request to **NOVA-CONDUCTOR** to get the information such as flavor, host ID
11. **NOVA-CONDUCTOR** picks the request and interacts with **NOVA-DATABASE** returns the required information sends to **NOVA-COMPUTE**.
12. **NOVA-COMPUTE** pick the required information from **NOVA-CONDUCTOR**.
13. **NOVA-COMPUTE** does the REST call by passing auth token to **GLANCE-API** to get the Image from Glance to download.
14. **GLANCE-API** validates the token with keystone and **NOVA-COMPUTE** gets the image.
15. Again **NOVA-COMPUTE** does the REST-call by passing auth token to **NETWORK API** to get the IP address for instance.
16. **NEUTRON-SERVER** validates the auth token with keystone and **NOVA-COMPUTE** get the network information
17. Same Again **NOVA-COMPUTE** does the REST call by passing auth token to Volume API to attache the volumes to instance
18. **CINDER-API** Validates the token with keystone and **NOVA-COMPUTE** get the block storage information
19. **NOVA-COMPUTE** generates data for hypervisor driver and executes there request on Hypervisor to **LAUNCH INSTANCE**.


**Below are the stages while running command**

| Status | Task | Steps |
---|---|---|
| Build| scheduling | 1-9 |
| Build | networking | 15,16 |
| Build | block device mapping | 17,18 |
| Build | spawning | 19 |
| Active | None |
