<p style="text-align: center" align="center">
    <a href="https://www.kubeinit.org"><img src="https://raw.githubusercontent.com/Kubeinit/kubeinit/master/images/logo.svg?sanitize=true" alt="The KUBErnetes INITiator"/></a>
</p>

**The KUBErnetes INITiator (The box)**


# Kubeinit In A Box 


- Main assembly: 05_assemblies/case.SLDASM
- Chasis parts: 00_chasis

## Reference Architecture:

- Latest revision: 2.1 BOM update [Carlos Camacho].

## Enclosure design

### External design, views and components organization.

The enclosure is designed as a rackable unit, using 7U. It tries to minimize the space used to deploy an up to 8-node cluster with redundancy for both power and networking.

#### Enclosure 3D renders

| | | |
|:-------------------------:|:-------------------------:|:-------------------------:|
|<img width="1604" alt="1" src="https://raw.githubusercontent.com/Kubeinit/box/master/07_pics/2/renders/desk.33.jpg">Title|<img width="1604" alt="2" src="https://raw.githubusercontent.com/Kubeinit/box/master/07_pics/2/renders/desk.34.jpg">Title|<img width="1604" alt="3" src="https://raw.githubusercontent.com/Kubeinit/box/master/07_pics/2/renders/desk.35.jpg">Title|
|<img width="1604" alt="4" src="https://raw.githubusercontent.com/Kubeinit/box/master/07_pics/2/renders/desk.36.jpg">Title|<img width="1604" alt="5" src="https://raw.githubusercontent.com/Kubeinit/box/master/07_pics/2/renders/desk.37.jpg">Title|<img width="1604" alt="6" src="https://raw.githubusercontent.com/Kubeinit/box/master/07_pics/2/renders/desk.38.jpg">Title|
|<img width="1604" alt="7" src="https://raw.githubusercontent.com/Kubeinit/box/master/07_pics/2/renders/desk.39.jpg">Title|<img width="1604" alt="8" src="https://raw.githubusercontent.com/Kubeinit/box/master/07_pics/2/renders/desk.39.jpg">Title|<img width="1604" alt="9" src="https://raw.githubusercontent.com/Kubeinit/box/master/07_pics/2/renders/desk.39.jpg">Title|

#### Mechanical views

| | | |
|:-------------------------:|:-------------------------:|:-------------------------:|
|<img width="1604" alt="1" src="https://raw.githubusercontent.com/Kubeinit/box/master/07_pics/2/parts/3.PNG">Title|<img width="1604" alt="2" src="https://raw.githubusercontent.com/Kubeinit/box/master/07_pics/2/parts/4.PNG">Title|<img width="1604" alt="3" src="https://raw.githubusercontent.com/Kubeinit/box/master/07_pics/2/parts/5.PNG">Title|
|<img width="1604" alt="4" src="https://raw.githubusercontent.com/Kubeinit/box/master/07_pics/2/parts/6.PNG">Title|<img width="1604" alt="5" src="https://raw.githubusercontent.com/Kubeinit/box/master/07_pics/2/parts/7.PNG">Title|<img width="1604" alt="6" src="https://raw.githubusercontent.com/Kubeinit/box/master/07_pics/2/parts/8.PNG">Title|
|<img width="1604" alt="7" src="https://raw.githubusercontent.com/Kubeinit/box/master/07_pics/2/parts/10.PNG">Title|<img width="1604" alt="8" src="https://raw.githubusercontent.com/Kubeinit/box/master/07_pics/2/parts/Capture.PNG">Title|<img width="1604" alt="9" src="https://raw.githubusercontent.com/Kubeinit/box/master/07_pics/2/parts/Capture.PNG">Title|

### Hardware components description

Here will be described the different HW combinations for configuring a Kubeinit cluster

The configuration of a Kubeinit chassis is dynamic enough to hold 8 different devices in
its bays. In the next picture you can see a general frontal view of different components that
can be allocated on each Kubeinit chassis. In the next image from left to right can be seen,
compute-node, gpu-bay, compute-node, gpu-bay, compute-node, storage-bay, compute-node, gpu-bay.

![](https://raw.githubusercontent.com/Kubeinit/box/master/07_pics/2/parts/7.PNG)

### Raspberry Pi remote management

There is a Raspberry Pi attached to the front cover of the chassis with the official 7” touch screen. This allows to:

- Control the 3 fan array in the back of the case.
- Show SNMP statistics from the cluster.
- Display cluster state.
- Reprovision the cluster using Ansible.
- Monitor temperature inside the chassis.
- Remote access to the cluster.
- Attach external Monitor/Keyboard/Mouse to a node with direct access to the cluster.

### Motherboards

There must be used Mini-ITX form factor motherboards on each Kubeinit bay. Some examples are the following.

Supermicro motherboards

Check Supermicro Mini-ITX motherboards[1], up-to 512GB RAM, up-to 16Cores, up-to 10GbE, refer only to SoC boards.

[1]:https://www.supermicro.com/support/resources/MB_matrix.php

Examples:

- https://www.supermicro.com/products/motherboard/Xeon/D/X10SDV-8C-TLN4F.cfm

### Networking

In a Kubeinit chassis can be allocated up to two networking switches, depending on the reference design they can be GbE or 10GbE and they can be configured in a redundant manner or not.

##### 10GbE configuration

Working on production ready configurations should use 10GbE by default.

##### Redundant scenarios

Redundancy should be provided from two networking switches based on the same model, in the case of a Kubeinit chassis there is a max width of 340mm.

##### Non redundant scenarios

For non redundant scenarios which can be used for non-critical production ready environments, there are multiple options depending on the size and number of ports.

- 16 ports 10GbE switch Netgear XS716E
- Raspberry Pi node
- Raspberry Pi display views
- Diagonal 1
- Diagonal 2
- Diagonal 3

### Additional storage

 There are three disk bays in each server pod, so the cluster can hold up to 24 2.5” disks. In addition if used the Asrock MB, can be used an additional 8 disk hot swap caddy with mini SAS ports.

### Expansion slots

Each node pod is able to hold a full size 16x PCIe card, this can be an additional GPU, FPGA board or any other custom board for specific use cases, for example an additional hot swap disk caddy.

There is a maximum number of additional devices that can be added, and this is based on two constraints, space and power consumption.

- Space: Up to 4 node pods with 1 external device each.
- Power: Each PCIe 16x slot can deliver up to 75w, if there is more power needed then there is the possibility to add 2 Flex ATX PSUs. Each 8-pin connector on the external cards theoretically can deliver up to 150w so forth, the 2 PSUs should allow to connect up to 2 external cards with 2 x 8-pin connector for high power demands.

## Deployment configurations

### Nodes configuration for deploying OpenShift

| Node type       | Quantity  | Description                                                                 |
| --------------- | --------- | --------------------------------------------------------------------------- |
| Bastion\*\*\*\* | 1\*\*\*\* | Deployment of the environment, Ansible playbooks, hardware management, etc. |
| Infrastructure  | 2         | OpenShift HAProxy, container registry, routing, etcd, logging, metrics.     |
| Master          | 3         | OpenShift API master, Kubernetes scheduler                                  |
| Compute         | 3         | Runs the application containers                                             |

**** The bastion host in this POC can be the raspberry pi node integrated in the front cover.

### Nodes configuration for deploying OpenStack

| Node type   | Quantity | Description |
| ----------- | -------- | ----------- |
| Controllers | 3        |             |
| Computes    | 4        |             |
| Undercloud  | 1        |             |
| \*\*\*      |          |             |

**** This node can be used also (raspberry pi node integrated in the front cover).
