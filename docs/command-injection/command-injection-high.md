# COMMAND INJECTION - HIGH
## ATTACK
### Objective
Exploit Command Injection in DVWA at High security level and identity what still works after stronger input sanitation and an expanded blacklist. 

### Steps
1. Log into DVWA at `http://Server-Address/dvwa`
2. Set the security level to **High**
3. Navigate to **Command Injections**
4. Test various bypass techniques to see what still works against the expanded blacklist. 

### Notes
- At High difficulty, DVWA add `trim()` and a much larger blacklist.
- Common characters like `;`, `&&`, `||`, `&`, `|`, `-`, `$`, `(`, `)`, and `` ` `` are filtered.

### Security Controls
#### What changed from Medium to High
// Medium
$target = $_REQUEST['ip'];

// High
$target = trim($_REQUEST['ip']);

**Expanded Blacklist at High:
```php
$substitutions = array(
    '||' => '',
    '&'  => '',
    '| ' => '',
    '-'  => '',
    '$'  => '',
    '('  => '',
    ')'  => '',
    '`'  => '',
);
```
### How this mitigates command injection
- trim() removes leading and trailing whitespace from the input.
- The expanded blacklist blocks more command chaining characters (||, &, |, -, $, (, ), `).
- This makes it much harder to inject commands using common separators and command substitution techniques.

### How It can be Bypassed
Even with this stronger blacklist, attackers can still attempt bypasses such as:
- Using alternative separators not in the blacklist (e.g., newlines %0a or encoded characters).
- Using encoding techniques (URL encoding, hex, Unicode).
- Exploiting logic flaws in how the blacklist is applied (e.g., spacing, case sensitivity, or combining allowed characters creatively).

## MONITOR
### Enable Audit Logging
Edit the ModSecurity config:
```bash
sudo nano /etc/modsecurity/modsecurity.conf
```
Find this line:
```apache
SecRuleEngine On
```
Change it to:
```apache
SecRuleEngine DetectionOnly
```
Then restart apache
```bash
sudo systemctl restart apache2
```
Live monitor with timestamps
```bash
sudo tail -f /var/log/apache2/modsec_audit.log | awk '{print strftime("%Y-%m-%d %H:%M:%S"), $0}'
```
This way, Modsecurity will log but not block any requests.

### Summary of the Bypass
- The blacklist includes '| ' => '' (pipe with a space).
- It does not block '|' (pipe without a space).
- Therefore, 127.0.0.1|whoami works, while 127.0.0.1 | whoami does not.

This is a classic incomplete blacklist bypass.

## MITIGATE
