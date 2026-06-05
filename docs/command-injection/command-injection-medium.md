# COMMAND INJECTION - MEDIUM LEVEL
## ATTACK
### Objective
Exploit Command Injection in DVWA at Medium security level and identify what still works after basic input sanitation is added. 

### Steps

1. Log into DVWA
2. Set the security level to **Medium**
3. Navigate to **Command Injection**
4. Test the following payloads:
    - 127.0.0.1 | whoami

### Notes
- At Medium difficulty, DVWA adds basic input sanitation. 
- Characters like `;` and `&&` are filtered.
- The pipe character (`|`) is not blocked, allowing limited command chaining. 

### Security Controls
#### What changed from Low to Medium
At Medium difficulty, DVWA introduced a basic blacklist to block certain command separators.
**Code Added:**
### DVWA Hardening
```php
    // Set blacklist
    $substitutions = array(
        '&&' => '',
        ';' => '',
    );
    // Remove any of the characters in the array (blacklist).
    $target = str_replace( array_keys( $substitutions ), $substitutions, $target);
```
### Remove any of the characters in the array (blacklist).
$target = str_replace( array_leys( $substitutions ), $substitutions, $target );

### Explanation
- Characters ; and && are now removed from user input.
- This is why payloads using ; and && stopped working at Medium.
- However, the pipe character (|) is not blocked.

## MONITOR
At medium difficulty, the pipe character (|) is still working. We all monitor it using ModSecurity.

### ModSecurity Audit Log Commands
**Search for the pip character**
```bash
sudo grep "|" /var/log/apache2/modsec_audit.log
```

**Search for common payloads**
```bash
sudo grep -E "whoami|id|ls|\|" /var/log/apache2/modsec_audit.log
```

**Real-time monitoring**
```bash
sudo tail -f /var/log/apache2/modsec_audit.log | grep -E ";|\||&&"
```
## MITIGATE
At Medium difficulty, DVWA applies a basic blacklist to remove dangerous command separators (; and &&).

How DVWA "Mitigates" at Medium:
- It uses a blacklist to strip ; and && from user input.
- This prevents basic command chaining using those characters.

How to Bypass:
- Use the pipe character (|) instead.
- Example: 127.0.0.1 | whoami

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
At Medium difficulty, basic input sanitazion is introduced, blocking common command separators like ; and &&. However, the pipe character (|) remains effective. PHP hardening combined with ModSecurity provides stronger protection in real environments.
