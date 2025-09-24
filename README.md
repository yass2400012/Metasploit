# Nmap Port Scanning & Metasploit Vulnerability scanning

<img src="Metasploit/metasploit.svg" alt="Metasploit" style="width:180px; height:auto;" />

## Summary:

Demonstrated TCP port enumeration using Nmap and quick UDP service discovery with Metasploit in an isolated lab VM.
Includes exact commands, module configurations, and captured outputs for reproducibility. documented the workflow and key findings (e.g., SMB, RDP, NetBIOS)..

## What I did

* Used `search portscan` to list Metasploit port-scanning modules.
* Inspected module options (`show options`) for `auxiliary/scanner/portscan/tcp`.
* Performed an Nmap TCP SYN scan from `msfconsole` for deeper service enumeration.
* Ran `auxiliary/scanner/discovery/udp_sweep` to identify UDP services (e.g., NetBIOS, DNS).
* Documented outputs and saved 7 screenshots showing the session in the VM.

---

## Commands (copy-paste friendly)

**List portscan modules**

```text
msf6 > search portscan
```

**Inspect module options**

```text
msf6 auxiliary(scanner/portscan/tcp) > show options
```

**Set a single host or range**

```text
# single host
set RHOSTS 10.201.99.202
# CIDR/range (example used in my notes)
set RHOSTS 10.201.99.202/24
# or from file
set RHOSTS file:/path/to/targets.txt
```

**Run a TCP portscan module**

```text
msf6 auxiliary(scanner/portscan/tcp) > set PORTS 1-1000
msf6 auxiliary(scanner/portscan/tcp) > set THREADS 10
msf6 auxiliary(scanner/portscan/tcp) > run
```

**Run Nmap from msfconsole**

```text
msf6 > nmap -sS 10.10.12.229
```

**Run UDP sweep**

```text
msf6 auxiliary(scanner/discovery/udp_sweep) > set RHOSTS 10.201.99.202/24
msf6 auxiliary(scanner/discovery/udp_sweep) > run
```

---

**Vulnerability scanning (VNC)**

Metasploit can quickly surface "low-hanging fruit" vulnerabilities once services are fingerprinted. For example, after identifying a VNC service during enumeration I used Metasploit's VNC scanner modules to check for weak or default authentication.


**Quick commands**
# list VNC scanners
```text
msf6 > use auxiliary/scanner/vnc/
```

# inspect a module
```text
msf6 auxiliary(scanner/vnc/vnc_login) > info
```

# run a VNC login scan (example)
```text
msf6 auxiliary(scanner/vnc/vnc_login) > set RHOSTS 10.201.99.202
msf6 auxiliary(scanner/vnc/vnc_login) > set PASS_FILE /path/to/wordlist.txt
msf6 auxiliary(scanner/vnc/vnc_login) > set THREADS 10
msf6 auxiliary(scanner/vnc/vnc_login) > run

```

**Notes:**

* vnc_login can test common/default passwords and use wordlists; tune BRUTEFORCE_SPEED, THREADS and STOP_ON_SUCCESS.

* Use info on any module to view options and behavior before running.

* Record and screenshot any successful logins or notable responses (screenshots included).

---


## Screenshots

*Stored in* `screenshots/` — filenames used in this repo:

**Metasploit update**:
![Metasploit update](Metasploit/Metasploit%20update.PNG)

**Search portscan**:
![Search portscan](Metasploit/Search%20portscan.PNG)

**Port scanning options**:
![Port scanning options](Metasploit/Port%20scanning%20options.PNG)

**Nmap port scan**:
![Nmap port scan](Metasploit/Nmap%20port%20scan.PNG)

**UDP service identification**:
![UDP service identification](Metasploit/UDP%20service%20identification.PNG)

**VNC scanning modules**:
![VNC scanning modules](Metasploit/VNC%20scanning%20modules.PNG)

**VNC login scan**:
![VNC login scan](Metasploit/VNC%20login%20scan.PNG)

Each image includes a short caption and the exact command used to create it.

---

## Key findings (example)

* `nmap -sS` identified several TCP services (e.g., 135, 139, 445, 3389) on the target host.
* `TCP scan revealed common Windows services (SMB, RDP, RPC).
* `UDP sweep detected NetBIOS service on UDP/137.
* `Showed differences between TCP and UDP scanning in terms of results and reliability.

---

## Skills

* **Port scanning & reconnaissance**: TCP/UDP discovery using Metasploit & Nmap.
* **Network enumeration**: Identifying open services (SMB, RDP, DNS, NetBIOS).
* **Tools**: Metasploit, Nmap, Linux/Windows lab VMs.
---

---

## Ethics & scope

All testing was performed in an isolated lab environment (VMs under my control). Scanning or attacking systems without explicit permission is unethical and illegal — mention this prominently if you re-use these materials.

---
