# Prerequisites
## Azure Virtual Machines
This tutorial builds a cluster with a single node,I used bastion,controller,worker as VM configuration.  
Supplements for multi-node are also included.  
When building with a single node, the ip of the controller node will be the public_ip of kubernetes.  
When building with multiple nodes, set up a Loadbalancer for the controller node. This LoadBalancer ip will be kubernetes public_ip.  
If you are connecting to a compute instance for the first time, change the permissions of your SSH key.

### Detailed conditions for Azure Virtual Machines.
OS. 
Linux (ubuntu 20.04).  
Size. 
Standard D2as v4 (2 vcpu 、8 GiB memory).  
Virtual Network: k8s-dev-vnet 

+ bastion.  
Name: k8s-dev-bastion  
Subnet: default (10.0.0.0/24)  
Public IP:　yes

+ controller.  
Name: k8s-dev-controller-0  
Subnet: controller_subnet (10.0.10.0/24)  
Public IP:　no

+ worker.  
Name: k8s-dev-worker-0  
Subnet: worker_subnet (10.0.20.0/24)  
Public IP:　no

Next: [Installing the Client Tools](https://github.com/Nagi-masashi/kubernetes/blob/main/docs/01-Installing%20the%20Client%20Tools.md)
