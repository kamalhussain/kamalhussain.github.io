---
layout: post
title:  "Using IP Namespaces"
date:   2015-11-17 16:16:01 -0600
categories: Linux, IP
permalink: /ip-namespaces/
---

Linux network namespaces allow creating isolated network stacks within a single instance of an operating system. Containers are typically built on network namespaces. In this post, we will take a look at configuring network namespaces on Linux. The setup is done on AWS and the configuration should look same on any Linux VM.

Two options were considered.
* Attach a physical interface to a namespace
* Use veth to connect host bridge to the namespace

## Setup
AWS supports creating VMs with two interfaces. This requires creating two distinct subnets. When you launch a VM, there will be an option to configure two interfaces and attach correct subnets. The interfaces on the configured VM should look like the following:

```
eth0      Link encap:Ethernet  HWaddr 02:28:6E:88:AB:02
          inet addr:10.1.0.74  Bcast:10.1.0.255  Mask:255.255.255.0
          inet6 addr: fe80::28:6eff:fe88:ab02/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:9001  Metric:1
          RX packets:46370 errors:0 dropped:0 overruns:0 frame:0
          TX packets:44265 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:9571595 (9.1 MiB)  TX bytes:5540547 (5.2 MiB)

eth1      Link encap:Ethernet  HWaddr 02:56:47:E1:1C:AA
          inet addr:10.1.1.83  Bcast:10.1.1.255  Mask:255.255.255.0
          inet6 addr: fe80::56:47ff:fee1:1caa/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:9001  Metric:1
          RX packets:5665 errors:0 dropped:0 overruns:0 frame:0
          TX packets:7058 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:291585 (284.7 KiB)  TX bytes:437119 (426.8 KiB)
```

## Option 1 - Attach a physical inteface to namespace
In this option, we will create namespace and attach a physical interface to it. Please follow these steps:

Create a namespace
```bash
sudo ip netns add blue
```
Assign physical interface to the namespace
```bash
sudo ip link set eth1 netns blue
```
Once the interface is moved, it will no longer be visible in the default namespace. Also, the IP of the interface needs to be reconfigured.

```bash
sudo ip netns exec blue ip addr add 10.1.1.83/24 dev eth1
```
Add the default route
```bash
sudo ip netns exec blue ip route add default via 10.1.1.1
```

Bring up the interface
```bash
sudo ip netns exec blue ip link set dev eth1 up
```

Display the network configuration
```
sudo ip netns exec blue ifconfig -a

```

To ssh to the namesapce, you need to start the sshd within the namespace using the following command
```
sudo ip netns exec blue /usr/sbin/sshd
```
With this setup, you should be able to ssh to 10.1.1.83, which will put you in the blue namespace.
The configuration of network namesapces don't persist over reboots so it is important to have startup scripts that will do the above setup.

## Option 2 - Use veth pair

In the option 1, a physical interface is attached to a namespace which means it cannot be used in any other namesapce. If we need to share the same interface across multiple namespaces, we can use Linux bridges along with veth pair.

### Setup
The Linux bridge needs to be configured on the interface that will be attached to a namespace. Please refer Linux documentation for configuring a bridge. The resulting setup should look like the following:

```
ubuntu@ns1:~$ ifconfig -a
br1       Link encap:Ethernet  HWaddr 02:95:dd:d6:43:56
          inet addr:10.1.1.51  Bcast:10.1.1.255  Mask:255.255.255.0
          inet6 addr: fe80::95:ddff:fed6:4356/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:24105 errors:0 dropped:0 overruns:0 frame:0
          TX packets:6935 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:1762700 (1.7 MB)  TX bytes:327914 (327.9 KB)
eth1      Link encap:Ethernet  HWaddr 02:95:dd:d6:43:56
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:6427 errors:0 dropped:0 overruns:0 frame:0
          TX packets:6489 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:306526 (306.5 KB)  TX bytes:309550 (309.5 KB)
          ...
```

```
ubuntu@ns1:~$ brctl show
bridge name	bridge id		STP enabled	interfaces
br1		8000.0295ddd64356	no		eth1
```
Now, create veth pair and tie it with both br1 and a peer interface in the namespace

```
sudo ip link add v-eth1 type veth peer name v-peer1
sudo ip link set v-eth1 master br1
sudo ip link set v-eth1 up
```
Create namespace and attach v-peer1 to it.
```
sudo ip netns add blue
sudo ip link set v-peer1 netns blue
sudo ip netns exec blue ip addr add 10.1.1.100/24 dev v-peer1
sudo ip netns exec blue ip link set v-peer1 up
sudo ip netns exec blue ip route add default via 10.1.1.51
sudo ip netns exec blue ip link set lo up
```

Just like in the previous step, you can start sshd in the newly created namespace. 



