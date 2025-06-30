---
title: Resources for Proxmox Cluster Subnet Changeover
draft: "true"
tags:
  - ProxmoxVE
  - virtualization
  - article
---

---
Various links and commands i collected when trying to disable clustering between 2x machines, may be of use to you! :)

---

https://www.reddit.com/r/Proxmox/comments/ttsstt/the_wifes_out_how_do_i_change_the_subnet_of_my/

https://forum.proxmox.com/threads/changing-the-ip-address-and-subnet-in-all-cluster-nodes.123426/

https://bookstack.dismyserver.net/books/documentation/page/how-to-change-the-ip-address-of-a-proxmox-clustered-node

https://www.youtube.com/watch?v=EQEE16JTlZs
https://www.youtube.com/watch?v=DlWXQrIhYQk&t=679s
https://www.youtube.com/watch?v=jKjGwhBwJ4c
https://www.youtube.com/watch?v=KWviA1O4dRs
https://www.youtube.com/watch?v=uh_3EiYpR4k

```
# Stop the cluster services
systemctl stop pve-cluster
systemctl stop corosync

# Mount the filesystem locally
pmxcfs -l

# Edit the network interfaces file to have the new IP information
# Be sure to replace both the address and gateway
vi /etc/network/interfaces

# Replace any host entries with the new IP addresses
vi /etc/hosts

# Change the DNS server as necessary
vi /etc/resolv.conf

# Edit the corosync file and replace the old IPs with the new IPs for all hosts
# :%s/192\.168\.1\./192.168.2./g   <- vi command to replace all instances
# BE SURE TO INCREMENT THE config_version: x LINE BY ONE TO ENSURE THE CONFIG IS NOT OVERWRITTEN
vi /etc/pve/corosync.conf

# Edit the known hosts file to have the correct IPs
# :%s/192\.168\.1\./192.168.2./g   <- vi command to replace all instances
/etc/pve/priv/known_hosts

# If using ceph, edit the ceph configuration file to reflect the new network
# (thanks u/FortunatelyLethal)
# :%s/192\.168\.1\./192.168.2./g   <- vi command to replace all instances
vi /etc/ceph/ceph.conf

# If you want to be granular... fix the IP in /etc/issue
vi /etc/issue

# Verify there aren't any stragglers with the old IP hanging around
cd /etc
grep -R '192\.168\.1\.' *
cd /var
grep -R '192\.168\.1\.' *

# Reboot the system to cleanly restart all the networking and services
reboot

# Referenced pages:
# - https://forum.proxmox.com/threads/change-cluster-nodes-ip-addresses.33406/
# - https://pve.proxmox.com/wiki/Cluster_Manager#_remove_a_cluster_node
```

Edit at the top:

This was fun. That's certainly a mix of honesty and sarcasm over the last few hours. So for anyone who is searching for a solution for similar in the future, here we go:

Taking some of the advise in the comments, I used the GUI to change the vmbr0 IP to what I wanted for each server, but did not apply it immediately. I updated the /etc/hosts file for the new IP as well. If I was going to do it again, I would do the next part and not reboot until having the script do it.

When it comes to corosync, I had some intuition that it would be more complicated than I expected. And I was certainly right. The correct way to do this seems to be:

1. From any healthy node, copy the current corosync file from /etc/pve/corosync.conf
    
2. Update that with the new IP addresses you are using. Increment the version!
    
3. Rsync this to your other nodes. I ended up making a folder with a few other cluster tools in it, one of which is "updateCorosync.sh"
    

systemctl stop pve-cluster && /usr/bin/pmxcfs -l
cp corosync.conf /etc/corosync/corosync.conf 
reboot

I copied the new corosync.conf version to the same folder and rsync'd that to the other machines. I rebooted those first, fought sync issues and partial cluster syncing/corosync syncing back to the original version until I did them all in unison.

While troubleshooting I also ssh'd between each machine and ran pvecm updatecerts, so that likely played into resuming communication.

Trying to do it machine by machine just caused issues.

So that's what I've got for yall. Time to go to cleanup and wind down. Thanks again for the comments.

Best,

Other notes:

This has been popular on a few posts I've commented on. This will remove cluster remnants from a node you are removing from a cluster. It's not what I'm doing tonight, but it's another helpful script to toss in a folder called ClusterTools or something if you keep scripts on your servers.

systemctl stop pve-cluster corosync
pmxcfs -l
rm -r /etc/corosync/*
rm /etc/pve/corosync.conf
killall pmxcfs
systemctl start pve-cluster

Original:

Hey team,

Like the title suggests, I have one of the rare evenings tonight where my wife will be out and I can have extended project time without fear of complaints or repercussions. Before starting with my current job or properly learning how to use VLANs, I was using a [10.0.0.0/17](https://10.0.0.0/17) subnet, using the third octet to define physical, servers, home automation, security, etc. My current company has a ton of 10.x.x.x subnets that get pushed when I connect to the VPN so I've been moving everything over to 192.168.x.0/24 VLANs with proper security so that I can still access my own resources in unison. That is, except, for the physical servers and primary LAN/DHCP.

I have a cluster of four Proxmox servers on the [10.0.0.0](https://10.0.0.0) subnet and I have been able to find some notes on how to change the IP of one at a time inside the same subnet, but is there any reasonable way to move them all to an entirely different subnet without breaking clustering (implying I will also have to backup/restore backups)? It's going to be a long evening of updating DNS records and whatnot so I'm trying to avoid unnecessary complication, but if that's the case so be it and I'll start the backups during lunch.

Thank you,

you will have to lose quorum, but I don't think it will break the cluster. you can set `pvecm expected 1` so that each server will operate normally without the other servers online. then just change the IP in /etc/network/interfaces and the corosync and other configs on each node I would assume.

I just did this for a 5 node cluster! I first changed the /etc/hosts, /etc/network/interfaces (or in the gui, but don't click apply), then in the corosync file. Then reboot the node and it'll all apply.

You will have to log into each node's shell and type "yes" to update the ssh keys. Alternatively you could just update the IP in the ssh file, but that former was easier.

Better Start by splitting cluster communications Into its own subnet, and independent from management ips.

The most critical thing is for every node to reach every other node by key--based SSH login after the IP change