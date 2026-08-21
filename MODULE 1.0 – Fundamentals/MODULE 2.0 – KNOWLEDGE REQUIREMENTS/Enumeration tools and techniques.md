  
## 1. Nmap – Network Scanning

**Why:** Nmap is your first look at any target. It tells you what's alive, what ports are open, what services are running, and what OS the machine might be. Without this, you're operating blind — you don't know what attack surface even exists.

**When:** This is almost always **the very first tool you run** on an engagement. Before touching anything else, you need a map of the network. You come back to Nmap repeatedly — once for host discovery, again for deep port scans, and again to run specific NSE scripts once you've identified interesting services.

##  2. enum4linux – SMB/Samba Enumeration

**Why:** SMB (port 445) is one of the richest sources of information on a Windows or Samba host. enum4linux specifically extracts users, groups, shares, password policies, and domain info — all things that feed directly into later attacks like password spraying, share access, or RID brute-forcing.

**When:** Run this **after Nmap reveals port 445 or 139 open** on a target. Especially valuable in internal network tests against Windows environments. If anonymous/null sessions are allowed (common in older or misconfigured systems), you can pull a full user list without any credentials at all.

##  3. WindowsEnum – Windows Local Enumeration

**Why:** Once you have a shell or RDP access on a Windows machine, you need to understand _exactly_ what you're working with — the OS version, patch level, local users, running services, and autoruns. This feeds your privilege escalation decision-making.

**When:** Used **immediately after gaining initial access** to a Windows host. You've landed a shell — now you need situational awareness before you make your next move. WindowsEnum automates the tedious manual commands so you can survey the host quickly without triggering too much noise.

##  4. JAWS – Just Another Windows Enum Script

**Why:** JAWS does very similar work to WindowsEnum but is specifically tuned to surface **privilege escalation vectors** — writable paths, unquoted service paths, interesting files, and credential clues. It's more privesc-focused than general situational awareness.

**When:** Run this **right after landing on a Windows box**, particularly when you're a low-privileged user trying to escalate. It's a PowerShell script, so it's easy to transfer and run in-memory. Many testers run both JAWS and WindowsEnum together for a more complete picture, or choose JAWS when privesc is the immediate goal.

# Nmap – Network ScanningLinux & Windows

nmap -sn 192.168.1.0/24Host discovery (ping sweep)Copied!

nmap -sV -sC -p- <target>Full port scan with scripts & version detectionCopied!

nmap -sS -T4 <target>SYN stealth scan (fast)Copied!

nmap -sU -p 53,161,162 <target>UDP scan on common portsCopied!

nmap -O <target>OS detectionCopied!

nmap -A -T4 <target>Aggressive scan (OS, version, scripts, traceroute)Copied!

nmap --script vuln <target>Run vulnerability detection scriptsCopied!

nmap --script smb-enum-users <target>Enumerate SMB usersCopied!

nmap --script smb-enum-shares <target>Enumerate SMB sharesCopied!

nmap --script ldap-search <target>LDAP enumerationCopied!

nmap -p 80,443 --script http-title <target>Grab HTTP titlesCopied!

nmap -oN output.txt -oX output.xml <target>Save output (normal + XML)Copied!

nmap -iL targets.txt -sVScan from host listCopied!

nmap -sV --script=banner <target>Banner grabbingCopied!

nmap --script dns-brute <target>DNS brute forceCopied!

## 🪟 enum4linux – SMB/Samba EnumerationLinux

enum4linux <target>Quick default enumerationCopied!

enum4linux -a <target>Full enumeration (all checks)Copied!

enum4linux -U <target>Enumerate usersCopied!

enum4linux -G <target>Enumerate groupsCopied!

enum4linux -S <target>Enumerate sharesCopied!

enum4linux -P <target>Get password policyCopied!

enum4linux -N <target>NetBIOS namesCopied!

enum4linux -R <target>RID cycling (user enumeration)Copied!

enum4linux -D <target>Domain enumerationCopied!

enum4linux -r <target>Enumerate users via RID cyclingCopied!

enum4linux -u 'admin' -p 'pass' -a <target>Authenticated full scanCopied!

enum4linux -W WORKGROUP -a <target>Specify workgroupCopied!

enum4linux -a -T 20 <target>Set RPC timeout to 20sCopied!

enum4linux-ng -A <target> -oJ out.jsonFull scan, JSON output (ng version)Copied!

enum4linux-ng -S <target>List shares only (ng version)Copied!

### 🪟 WindowsEnum – Windows Local EnumerationWindows

powershell -ep bypass .\WindowsEnum.ps1Run with execution policy bypassCopied!

.\WindowsEnum.ps1Basic local system enumerationCopied!

WindowsEnum checks: System infoHostname, OS, architecture, patchesCopied!

WindowsEnum checks: Local users and groupsLocal user & group enumerationCopied!

WindowsEnum checks: Running processes and servicesActive processes & services listingCopied!

WindowsEnum checks: Scheduled tasksScheduled task enumerationCopied!

WindowsEnum checks: Network interfaces and connectionsNetwork config & active connectionsCopied!

WindowsEnum checks: Installed softwareInstalled application listingCopied!

WindowsEnum checks: Shared foldersNetwork share enumerationCopied!

WindowsEnum checks: Registry autorunsAutorun entry detectionCopied!

WindowsEnum checks: PowerShell history and transcriptPS history & transcript filesCopied!

WindowsEnum checks: AV/EDR product detectionAntivirus & EDR identificationCopied!

#### 🦅 JAWS – Just Another Windows Enum ScriptWindows

powershell.exe -ExecutionPolicy Bypass -File .\jaws-enum.ps1Run JAWSCopied!

powershell.exe -ExecutionPolicy Bypass -File .\jaws-enum.ps1 -OutputFilename JAWS-Enum.txtSave output to fileCopied!

JAWS checks: System info (OS, arch, hostname, domain)System information gatheringCopied!

JAWS checks: Network configuration (IPs, DNS, routes)Network config enumerationCopied!

JAWS checks: Running processesActive process listingCopied!

JAWS checks: Scheduled tasksScheduled task enumerationCopied!

JAWS checks: Local users and groupsUser & group enumerationCopied!

JAWS checks: Installed software and patchesSoftware & patch listingCopied!

JAWS checks: Writeable directories and filesWritable path discoveryCopied!

JAWS checks: AV/EDR detectionSecurity product identificationCopied!

JAWS checks: Mapped drives and sharesMapped drive enumerationCopied!

JAWS checks: Interesting files (passwords, configs)Sensitive file discoveryCopied!

JAWS checks: Potential privilege escalation vectorsPrivesc vector identificationCopied!



##### 🌐 Network EnumerationLinux & Windows

netdiscover -r 192.168.1.0/24ARP host discoveryCopied!

arp-scan --localnetARP scan local networkCopied!

masscan -p1-65535 <target> --rate=1000Fast full port scanCopied!

netstat -tulnp / netstat -anoActive connections and listeners (Linux / Windows)Copied!

ss -tulnpSocket statistics (Linux)Copied!

route -n / route printRouting table (Linux / Windows)Copied!

ip neigh showARP cache (Linux)Copied!

arp -aARP cache (Windows/Linux)Copied!

nbtscan 192.168.1.0/24NetBIOS scanCopied!

unicornscan <target>:1-65535Fast async port scanCopied!

onesixtyone -c community.txt -i hosts.txtSNMP community string bruteCopied!

snmpwalk -v2c -c public <target>SNMP walkCopied!

snmp-check <target>SNMP info dumpCopied!

###### 🌍 DNS EnumerationLinux

dnsrecon -d <domain> -t stdStandard DNS reconCopied!

dnsrecon -d <domain> -t axfrZone transfer attemptCopied!

dnsenum <domain>DNS enumeration + zone transferCopied!

dig axfr <domain> @<nameserver>Zone transfer (Linux)">Copied!

host -t ns <domain>List nameserversCopied!

host -t mx <domain>List mail serversCopied!

nslookup -type=any <domain>All DNS recordsCopied!

fierce --domain <domain>DNS brute forceCopied!

gobuster dns -d <domain> -w /usr/share/wordlists/subdomains.txtDNS subdomain brute forceCopied!

amass enum -d <domain>Passive/active subdomain enumCopied!

theHarvester -d <domain> -b allEmail, subdomain harvestingCopied!

🌐 Web EnumerationLinux

gobuster dir -u http://<target> -w /usr/share/wordlists/dirb/common.txtDirectory brute forceCopied!

gobuster dir -u http://<target> -w wordlist.txt -x php,html,txtWith extensionsCopied!

ffuf -w wordlist.txt -u http://<target>/FUZZDirectory fuzzingCopied!

ffuf -w wordlist.txt -u http://<target>/FUZZ -fc 404Filter 404sCopied!

nikto -h http://<target>Web vulnerability scannerCopied!

nikto -h http://<target> -output nikto.txtSave nikto outputCopied!

wfuzz -c -w wordlist.txt http://<target>/FUZZWeb fuzzerCopied!

whatweb http://<target>Technology fingerprintingCopied!

curl -I http://<target>HTTP headers (banner grab)Copied!

wafw00f http://<target>WAF detectionCopied!

dirb http://<target> /usr/share/wordlists/dirb/common.txtDirectory brute with dirbCopied!

feroxbuster -u http://<target> -w wordlist.txtRecursive directory brute forceCopied!

python3 -m http.server 80Serve files (attacker machine)Copied!

🗂️ SMB EnumerationLinux

smbclient -L //<target>/ -NList shares (anonymous)Copied!

smbclient //<target>/share -NConnect to share (anonymous)Copied!

smbmap -H <target>List shares with permissionsCopied!

smbmap -H <target> -u 'user' -p 'pass'Authenticated share mapCopied!

crackmapexec smb <target>Quick SMB infoCopied!

crackmapexec smb <target> -u 'user' -p 'pass' --sharesEnum shares (auth)Copied!

crackmapexec smb <target> -u 'user' -p 'pass' --usersEnum users (auth)Copied!

crackmapexec smb <target> -u 'user' -p 'pass' --groupsEnum groups (auth)Copied!

rpcclient -U "" -N <target>Anonymous RPC connectionCopied!

rpcclient -U "user%pass" <target>Authenticated RPCCopied!

rpcclient $> enumdomusersEnumerate domain users (inside rpcclient)Copied!

rpcclient $> enumdomgroupsEnumerate domain groupsCopied!

rpcclient $> querydominfoQuery domain infoCopied!

impacket-smbclient <target>SMBclient via ImpacketCopied!

nmap --script smb-vuln-ms17-010 <target>Check EternalBlueCopied!

🏰 Active Directory EnumerationLinux & Windows

bloodhound-python -u user -p pass -d domain.local -ns <dc-ip> -c AllBloodHound data collectionCopied!

SharpHound.exe -c AllBloodHound (Windows, C# ingestor)Copied!

ldapsearch -x -H ldap://<dc-ip> -b "dc=domain,dc=local"Anonymous LDAP searchCopied!

ldapsearch -x -H ldap://<dc-ip> -D "user@domain.local" -w pass -b "dc=domain,dc=local"Auth LDAP searchCopied!

python3 GetADUsers.py -all domain.local/user:passDump all AD users (Impacket)Copied!

python3 GetUserSPNs.py domain.local/user:passKerberoastable SPNsCopied!

python3 GetNPUsers.py domain.local/ -usersfile users.txt -no-passAS-REP roastingCopied!

crackmapexec smb <dc-ip> -u user -p pass --active-usersActive AD usersCopied!

net user /domainList domain users (Windows)Copied!

net group /domainList domain groups (Windows)Copied!

net group "Domain Admins" /domainDomain Admins members (Windows)Copied!

Get-ADUser -Filter * | Select Name,SamAccountNameAD users (PowerShell)Copied!

Get-ADComputer -Filter * | Select Name,OperatingSystemAD computers (PowerShell)Copied!

powerview: Get-DomainUserPowerView: dump all domain usersCopied!

powerview: Get-DomainGroupPowerView: dump all domain groupsCopied!

powerview: Find-LocalAdminAccessPowerView: find machines where current user is adminCopied!

powerview: Get-DomainTrustPowerView: enumerate domain trustsCopied!

🔑 SNMP EnumerationLinux

snmpwalk -v1 -c public <target>SNMPv1 walk with 'public' communityCopied!

snmpwalk -v2c -c public <target>SNMPv2c walkCopied!

snmpwalk -v2c -c public <target> 1.3.6.1.2.1.25.4.2.1.2Running processes via SNMPCopied!

snmp-check <target>Detailed SNMP enumerationCopied!

onesixtyone -c /usr/share/doc/onesixtyone/dict.txt <target>Community string bruteCopied!

snmpbulkwalk -v2c -c public <target>Bulk walk (faster)Copied!

hydra -P community.txt <target> snmpBrute force SNMP communitiesCopied!

nmap -sU -p 161 --script=snmp-brute <target>Nmap SNMP bruteCopied!

##### 📋 Local System Enumeration – LinuxLinux

uname -aKernel versionCopied!

cat /etc/os-releaseOS infoCopied!

hostname && id && whoamiHost and current userCopied!

cat /etc/passwdUser accountsCopied!

cat /etc/groupGroup membershipsCopied!

lastLast loginsCopied!

wLogged in usersCopied!

ps auxRunning processesCopied!

df -hDisk usageCopied!

mountMounted filesystemsCopied!

find / -name "*.conf" 2>/dev/nullConfig filesCopied!

find / -name "id_rsa" 2>/dev/nullSSH private keysCopied!

cat ~/.ssh/known_hostsKnown SSH hostsCopied!

historyCommand historyCopied!

crontab -l && cat /etc/crontabCron jobsCopied!

iptables -LFirewall rulesCopied!

###### Local System Enumeration – WindowsWindows

systeminfoFull system infoCopied!

whoami /allFull user info + privilegesCopied!

net userLocal usersCopied!

net localgroupLocal groupsCopied!

net localgroup administratorsAdmin group membersCopied!

ipconfig /allNetwork configCopied!

netstat -anoActive connectionsCopied!

tasklist /SVCRunning processes with servicesCopied!

wmic product get name,versionInstalled softwareCopied!

wmic os getOS infoCopied!

wmic qfe list briefInstalled patchesCopied!

reg query HKLM\System\CurrentControlSet\ServicesInstalled servicesCopied!

dir /a /s C:\*.txt 2>nulSearch for text filesCopied!

dir /a /s C:\*pass* 2>nulSearch for password filesCopied!

findstr /si password *.txt *.xml *.iniSearch for passwords in filesCopied!

cmdkey /listStored credentials