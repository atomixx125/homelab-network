# homelab-network
A network I made using some tech lying around. 

# homelab enterprise network project

## overview

this project is a segmented enterprise-style home network built using a cisco catalyst switch, multiple physical machines, and virtualized infrastructure. the goal is to simulate real-world enterprise environments for learning:

- network engineering (vlans, switching, routing)
- active directory environments
- security operations (siem, logging, detection)
- penetration testing / attack simulation
- system administration across linux and windows

this lab is intentionally designed to model a small corporate network with separated trust zones, monitoring, and an isolated attack environment.

---

## high-level architecture
  internet
      |
    router
      |
cisco catalyst switch
      |
  -----------------------------------------    
  |   | |       |                         |     
pi thinkpad windows all-in-one vm host attack lab
(admin) laptop (server) (services) (isolated)




---

## vlan design

| vlan | name        | purpose |
|------|------------|---------|
| 10   | mgmt       | admin access, ssh, control plane |
| 20   | servers    | infrastructure services (ad, dns, siem) |
| 30   | users      | client machines and normal usage |
| 40   | attack     | isolated penetration testing environment |
| 50   | security   | logging, monitoring, detection tools |
| 99   | quarantine | misconfigured or unsafe devices |

---

## device roles

### cisco catalyst switch
- layer 2 segmentation (vlans)
- port-based network separation
- optional trunking for advanced setups

---

### raspberry pi
role: network infrastructure + control plane

services:
- dns (pihole or unbound)
- vpn (wireguard)
- ssh bastion host
- log relay (syslog)

---

### thinkpad (arch linux / endeavourOS)
role: operator / admin workstation

used for:
- ssh into all systems
- network monitoring
- penetration testing tools
- incident investigation

---

### windows laptop
role: virtualization host

runs:
- windows server (active directory domain controller)
- windows client vm(s)
- kali linux vm
- ubuntu server vm

---

### all-in-one system (headless server)
role: core infrastructure node

services:
- docker workloads
- internal web services
- optional SIEM stack
- logging collection
- dns or reverse proxy services

---

## core services

### active directory domain
- domain: `corp.local`
- services:
  - kerberos authentication
  - ldap directory services
  - group policy management

---

### dns
- centralized or pi-hole based dns
- internal name resolution for lab systems

---

### siem / logging

tools:
- wazuh
- syslog aggregation
- endpoint log collection

purpose:
- centralized security monitoring
- alerting and detection engineering

---

### network monitoring

tools:
- zeek
- suricata

purpose:
- traffic analysis
- intrusion detection
- protocol visibility (dns, smb, http, kerberos)

---

## security design

### segmentation model
- users cannot access management vlan
- attack network is fully isolated
- only controlled traffic between servers and clients

### trust boundaries
- mgmt (vlan 10) has full access
- servers (vlan 20) serve internal services only
- users (vlan 30) are restricted clients
- attack (vlan 40) is fully sandboxed
- security (vlan 50) has read-only visibility across network

---

## attack simulation environment

the attack vlan includes tools for:
- network scanning
- active directory attacks
- credential abuse simulation
- exploit testing
- malware execution in isolated conditions

this environment is intentionally segmented to prevent lateral movement into production or management systems.

---

## common incidents and troubleshooting

### issue: no internet access
possible causes:
- incorrect vlan assignment
- dhcp failure
- dns misconfiguration

resolution:
- verify switch port vlan
- check dhcp leases
- test dns resolution

---

### issue: cannot join domain
possible causes:
- dns misconfigured
- time drift (kerberos failure)
- firewall blocking ldap/kerberos

resolution:
- set dns to domain controller
- synchronize time
- verify connectivity to dc

---

### issue: vm not reachable on lan
possible causes:
- nat mode instead of bridged networking
- incorrect ip configuration

resolution:
- ensure bridged adapter is enabled
- verify ip subnet consistency

---

### issue: logs not appearing in SIEM
possible causes:
- agent not installed
- firewall blocking ports
- incorrect configuration

resolution:
- verify wazuh agent status
- check connectivity to siem server
- confirm log forwarding settings

---

## tools used

- cisco catalyst switch
- raspberry pi (raspberry pi os)
- windows server
- windows 10/11
- kali linux
- ubuntu server
- hyper-v / vmware / virtualbox
- wazuh
- zeek
- suricata
- wireguard
- pihole

---

## learning outcomes

this project develops practical skills in:

- enterprise network segmentation (vlans)
- active directory administration and exploitation
- security monitoring and detection engineering
- packet analysis and traffic inspection
- linux and windows system administration
- virtualization and lab orchestration
- incident troubleshooting and root cause analysis

---

## future improvements

- pfSense firewall integration
- elastic stack for advanced logging
- automated attack detection rules
- infrastructure as code (ansible/terraform)
- phishing simulation environment
- containerized service orchestration

---

## notes

this environment is intentionally designed for experimentation, failure, and iterative learning. systems may be reset, broken, and rebuilt frequently as part of the learning process.

---

## topology diagram

(placeholder - insert draw.io or png export here)
