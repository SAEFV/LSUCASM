# LSUCASM
what to do with my school server

# Linux Server Use Cases: Authentication, Security Monitoring & Networking

## 1. OpenLDAP + Samba for Centralized Authentication

### Why Use OpenLDAP + Samba?
✅ Centralized user management (Linux & Windows support).  
✅ Single Sign-On (SSO) for multiple Linux servers.  
✅ Samba provides AD-like features for file sharing & authentication.  

### How It Works
- **OpenLDAP**: Stores user accounts, groups, and authentication data.  
- **Samba**: Uses LDAP as its backend for authentication, allowing Windows clients to join the domain.  
- **SSSD (System Security Services Daemon)**: Enables Linux machines to authenticate against LDAP.  
- **Kerberos (Optional)**: For ticket-based authentication (like Windows AD).  

### Setup Steps (Simplified)
#### Install OpenLDAP Server
```bash
sudo apt update && sudo apt install slapd ldap-utils
```
- Configure OpenLDAP (`dpkg-reconfigure slapd`).  
- Use `ldapadd` to add user accounts.  

#### Install Samba & Configure it to Use LDAP
```bash
sudo apt install samba smbldap-tools
```
- Configure `smb.conf` to use LDAP as a backend.  
- Use `smbldap-config` to integrate with OpenLDAP.  

#### Join Linux & Windows Clients
- On Linux, install **SSSD** and configure it to authenticate against LDAP.  
- On Windows, configure it to use **Samba** as a domain controller.  

---

## 2. SIEM & Security Monitoring

### Why Use a SIEM?
✅ Centralized log collection & monitoring.  
✅ Intrusion detection & alerts.  
✅ Threat intelligence integration.  

### Popular Open-Source SIEM Solutions
#### **Wazuh (Best for Security Monitoring)**
- Monitors host-based security (logins, changes, processes).  
- Integrates with Elasticsearch & Kibana for visualization.  
- Detects malware, rootkits, and policy violations.  

#### **Elastic Stack (ELK: Elasticsearch, Logstash, Kibana)**
- Best for log analysis & visualization.  
- Collects logs from multiple sources (syslog, firewall, applications).  
- Can be used with Wazuh for security alerts.  

#### **Suricata (IDS/IPS)**
- Monitors network traffic for suspicious activity.  
- Can detect & block attacks in real-time.  

#### **Zeek (Bro IDS)**
- Network security monitoring.  
- Captures detailed network traffic logs.  

### Setup Steps
#### Install Wazuh (SIEM agent + server)
```bash
sudo apt install wazuh-manager
```
- Configure Wazuh to collect logs from servers, firewalls, and endpoints.  
- Install the Wazuh agent on Linux clients.  

#### Set Up Log Forwarding with rsyslog
```bash
*.* @amdm01:514
```

#### Use Kibana for Log Visualization
- Install **Elasticsearch + Kibana** for dashboards.  

#### Deploy Suricata for Intrusion Detection
```bash
sudo apt install suricata
```

---

## 3. DNS & Network Services

### 1. Pi-hole + Unbound (Local DNS & Ad Blocking)
✅ Pi-hole blocks ads, trackers, and malware domains.  
✅ Unbound runs as a local DNS resolver, improving speed & privacy.  

### Setup Steps
#### Install Pi-hole
```bash
curl -sSL https://install.pi-hole.net | bash
```
- Set it as your network's primary DNS server.  
- Blocks ads on all devices (no need for browser extensions).  

#### Install & Configure Unbound for Local DNS Resolution
```bash
sudo apt install unbound
```
- Configures a recursive DNS server to resolve queries without relying on Google or Cloudflare.  

### 2. DHCP & Local Networking
Instead of using your router’s DHCP, you can set up a Linux-based DHCP server for better control over IP assignments.  

#### Install & Configure DHCP Server
```bash
sudo apt install isc-dhcp-server
```

Edit `/etc/dhcp/dhcpd.conf`:
```bash
subnet 192.168.1.0 netmask 255.255.255.0 {
  range 192.168.1.100 192.168.1.200;
  option routers 192.168.1.1;
  option domain-name-servers 192.168.1.2, 8.8.8.8;
}
```
- This will assign IPs from **192.168.1.100 - 192.168.1.200** with **Pi-hole (192.168.1.2)** as the primary DNS.  

---

## Final Thoughts
With your Linux server, you can:
✅ Run **OpenLDAP + Samba** for authentication.  
✅ Use **SIEM tools (Wazuh, ELK, Suricata)** for security monitoring.  
✅ Host **network services (Pi-hole, DHCP, Unbound)** for improved security & performance.  
