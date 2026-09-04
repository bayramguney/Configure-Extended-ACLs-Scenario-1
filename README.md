# Configure-Extended-ACLs-Scenario-1

# Packet Tracer – Configure Extended ACLs (Scenario 1)

## Overview

This lab demonstrates how to configure, apply, and verify both **Extended Numbered ACLs** and **Extended Named ACLs** in Cisco Packet Tracer. Extended ACLs filter traffic based on source and destination IP addresses, protocols, and port numbers to enforce network security policies.

## Objectives

* Configure an Extended Numbered ACL.
* Configure an Extended Named ACL.
* Apply ACLs to the correct router interfaces.
* Verify ACL functionality using ping, FTP, and HTTP tests.

## Network Topology

* **Router:** R1
* **Server**
* **PC1**
* **PC2**

### Networks

* **Server LAN:** 172.22.34.0/26
* **PC1 LAN:** 172.22.34.64/27
* **PC2 LAN:** 172.22.34.96/28

## Security Policies

### PC1 Network

* Allow **FTP** access to the Server.
* Allow **ICMP (Ping)** to the Server.
* Deny all other traffic.

### PC2 Network

* Allow **HTTP (Web)** access to the Server.
* Allow **ICMP (Ping)** to the Server.
* Deny all other traffic.

## Configuration Summary

### Extended Numbered ACL (100)

```plaintext
access-list 100 permit tcp 172.22.34.64 0.0.0.31 host 172.22.34.62 eq ftp
access-list 100 permit icmp 172.22.34.64 0.0.0.31 host 172.22.34.62

interface GigabitEthernet0/0
 ip access-group 100 in
```

### Extended Named ACL (HTTP_ONLY)

```plaintext
ip access-list extended HTTP_ONLY
 permit tcp 172.22.34.96 0.0.0.15 host 172.22.34.62 eq www
 permit icmp 172.22.34.96 0.0.0.15 host 172.22.34.62
 exit

interface GigabitEthernet0/1
 ip access-group HTTP_ONLY in
```

## Verification Commands

```plaintext
show access-lists
show running-config
show ip interface
```

## Expected Results

| Test                | Expected Result |
| ------------------- | --------------- |
| PC1 → Server (Ping) | ✅ Success       |
| PC1 → Server (FTP)  | ✅ Success       |
| PC1 → PC2           | ❌ Blocked       |
| PC2 → Server (Ping) | ✅ Success       |
| PC2 → Server (HTTP) | ✅ Success       |
| PC2 → Server (FTP)  | ❌ Blocked       |

## Skills Learned

* Extended Numbered ACL configuration
* Extended Named ACL configuration
* Filtering by IP address, protocol, and port number
* Wildcard mask calculation
* Applying ACLs to interfaces
* Verifying ACL operation with Cisco IOS commands
* Testing network access using Ping, FTP, and HTTP

## Conclusion

This lab demonstrates how Extended ACLs provide granular traffic filtering by controlling specific protocols and services. Proper ACL placement near the traffic source improves network security while ensuring authorized services remain accessible.
