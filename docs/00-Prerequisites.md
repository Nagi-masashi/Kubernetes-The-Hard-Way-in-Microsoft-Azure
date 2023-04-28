# Prerequisites
## Azure Virtual Machines
This tutorial builds a cluster with a single node,I used bastion,controller,worker as VM configuration.  
Supplements for multi-node are also included.  
When building with a single node, the ip of the controller node will be the public_ip of kubernetes.  
When building with multiple nodes, set up a Loadbalancer for the controller node. This LoadBalancer ip will be kubernetes public_ip.

### Detailed conditions for Azure Virtual Machines.
OS. 
Linux (ubuntu 20.04).  
Size. 
Standard D2as v4 (2 vcpu 、8 GiB memory).  

+ bastion.  
Name. k8s-dev-bastion.  
Public IP　yes

+ controller.  
Name. k8s-dev-controller-0.  
Public IP　no

+ worker.  
Name. k8s-dev-worker-0.  
Public IP　no

Next: [Installing the Client Tools](https://github.com/Nagi-masashi/kubernetes/blob/main/docs/01-Installing%20the%20Client%20Tools.md)
