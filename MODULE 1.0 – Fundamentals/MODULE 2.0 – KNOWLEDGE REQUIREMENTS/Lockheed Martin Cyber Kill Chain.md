Identifies what adversaries must complete in order to accomplish their objective.

**Reconnaissance** - Studying the target before doing anything overt
Steps that go into this would include:
- Identify the target
- Gather information  ( Collect emails, usernames, tech stack, ip ranges, exposed services )
- Map weaknesses (Look for outdated systems. misconfigurations)

**Weaponization** - Creating a malicious payload and using an exploit to make the delivery

**Delivery** - Transmission of the payload to the victim via Phishing emails, Drive-by downloads, USB drops, social engineering, and **Exploiting Exposed Services**

**Exploitation** - The delivered payload is activated by exploiting a vulnerability via software vulnerabilities, user execution, Misconfigurations, and script or code execution

**Installation** -  This is where an attacker establishes a foothold in the victims network by installing malware or using methods to ensure persistence like modifying:
- registry run keys
- scheduled tasks
- services
- startup folder entries
- browser extensions
- kernel-level implants
- security tools
- firewall rules
- accounts

**Command and Control** - When the installed malware enables a connection to the attackers infrastructure to control the compromised system. Comms look like:
- outbound network connections
	- http/https
	- dns tunneling
	- encrypted channels
	- custom protocols
- Beaconing - 

**Actions on Objective** - 