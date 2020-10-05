# Project1
Group work for Computer Systems Security, project 1.

## Introduction

Trevor Chaney: Assisted in data collection for all tasks and wrote the closing section for section V. The group had three weekly meetings to work on tasks for ~2 hours on Mondays, Wednesdays, and Fridays.

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

In closing we will discusse how the security policy could be inplimented on the company's server (A.1) and workstations (A.2) and further whether or not the security policy, as is, could ensure that classified data will **NOT** be disclosed to computers external to the company (computers on network B)

### A) Show iptables rules to enforce the security policy in A.1

The following is an implimentation for the iptables rules that would enforce the security policy in the company's server (represented as A.1). An explination of these rules and why the are used follows after the illistration.

```bash
1: $ iptables -A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
2: $ iptables -A INPUT -p tcp -s 172.16.0.101/24 --dport 80 -j ACCEPT
3: $ iptables -A INPUT -p tcp -s 10.0.0.1/24 --dport 80 -j ACCEPT
4: $ iptables -A INPUT -p tcp -s 172.16.0.101/24 --dport 22 -j ACCEPT
5: $ iptables -A INPUT -p icmp -s 172.16.0.101/24 -j ACCEPT
6: $ iptables -A INPUT -s 172.16.0.101/24 -j DROP
```

#### Explination

The issues found with the security policy, as is, in regards to the company's server were found in policy rule 'b', "The server provides only SSH and web service to the workstations." The reason that this policy could not be implimented on the network router (R) is because these connections skipped the router entirly at the switch level and 'R' was unable to prevent their connection. In fact, 'R' never saw that they were talking to eachother. So, the above policy should be implimented on 'A.1'. A line-by-line explination follows:

- line 1: This command will **Append** to **INPUT** to **ACCEPT** all related and established rules.
- line 2: This command will **Append** to **INPUT** to **ACCEPT** all traffic on port **80** from the workstation's IP, there by enabling **Web service**.
- line 3: This command will **Append** to **INPUT** to **ACCEPT** all traffic on port **80** from the external network 'B', there by enabling **Web service**.
- line 4: This command will **Append** to **INPUT** to **ACCEPT** all traffic on port **22** from the workstation's IP, there by enabling **SSH**. 
- line 5: This command will **Append** to **INPUT** to **ACCEPT** all **icmp** protocall requests, this will allow for responses to connection.
- line 6: This command will **DROP** all other connection attempts to the server.

### B) Show iptables rules to enforce the security policy in A.2

The following is an implimentation for the iptables rules that would enforce the security policy in the company's server (represented as A.2). An explination of these rules and why the are used follows after the illistration.

```bash
1: $ iptables -A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
2: $ iptables -A INPUT -j DROP
```

### Explination

The issues found with teh security policy as it is in regards to the company's workstations were found in the policy rule 'e', "The workstations can access the services hosted by the server." This rule cannot be enforced by the router 'R' as again it wouldn't see these connections and so it must be explicitely stated that workstations 'A.2' can accept traffic coming from the server 'A.1'. The following is an explination of the rules that would enforce that a workstation can access services in network 'A' that are provided by the server. The following is a line-by-line explination:

- line 1: This command will **Append** to **INPUT** to **ACCEPT** all related and established rules, this includes services provided by the server.
- line 2: This command will **DROP** all other traffic.

### C) Discussion of how the security policy could ensure non-Disclosure

Non-disclosure in regards to the security palicy as it is written