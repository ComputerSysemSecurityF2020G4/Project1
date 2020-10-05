# Project1
Group work for Computer Systems Security, project 1.

## Introduction

## Network Diagnosis, Task II

### A) NMap commands to scan the computers and the service ports
### B) Discovered IP's and services in Network A and B
#### 1) Run NMap in A.2 on Network A

`nmap 172.16.0.0/24`

Scan all computers and services in Network A. Record the identified computers and services.

![nmap output for task 2 on network A](./images/nmap_A.png)


#### 2) Run NMap in A.2 on Network B

`nmap 10.0.0.0/24`

Scan all computers and services in Network B. Record the identified computers and services.

![nmap output for task 2 on network B](./images/nmap_Bpart1.png)

![nmap output for task 2 on network B part 2](./images/nmap_Bpart2.png)

### C) Wireshark results of checking the web service between B.1 and A.1, and, A.2 and A.1
#### B.1 and A.1
#### A.2 and A.1

### D) Wireshark results of checking the ping between B.1 and A.1, and, A.2 and A.1
#### B.1 and A.1
#### A.2 and A.1

## Impliment Security Policy, Task III
### A) Access Control Matrix
| Name:             | A.1 (server)    | A.2 (workstations)| B (external) |
|-------------------|-----------------|-------------------|--------------|
| A.1 (server)      | -               | ping              | ping         |
| A.2 (workstations)| ssh, http, ping | ping              | http, ping   |
| B (external)      | http            | -                 | -            |
### B) Issues with Policy Compliance 
### C) iptables Rules in R
![iptables rules in R](./images/iptables.png)

## Test Implimentation of The Security Policy, Task IV
### A) Show NMap Results of Exposure of Network A
### B) Wireshark results of checking the web service between B.1 and A.1, and, A.2 and A.1
#### B.1 and A.1, stating whether or not web service is allowed between computers.
#### A.2 and A.1, stating whether or not web service is allowed between computers.
### C) Wireshark results of checking the ping between B.1 and A.1, and, A.2 and A.1
#### B.1 and A.1, stating whether or not ping is allowed between computers.
#### A.2 and A.1, stating whether or not ping is allowed between computers.

## Closing
### A) Show iptables rules to enforce the security policy in A.1

```
iptables -A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
iptables -A INPUT -p tcp -s 172.16.0.101/24 --dport 80 -j ACCEPT
iptables -A INPUT -p tcp -s 172.16.0.101/24 --dport 22 -j ACCEPT
iptables -A INPUT -p icmp -s 172.16.0.101/24 -j ACCEPT
```

### B) Show iptables rules to enforce the security policy in A.2

```
iptables -A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
iptables -A INPUT -j DROP
```
### C) Discussion of how the security policy could ensure non-Disclosure
