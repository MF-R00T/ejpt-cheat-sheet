# ejpt-cheat-sheet
![image](https://user-images.githubusercontent.com/112498030/204037344-1d0b543f-9bf1-4419-9590-50fe589f3370.png)

>"The eLearnSecurity Junior Penetration Tester (eJPT) is a 100% practical certification on penetration testing and information security essentials."

This is a record of Methodology, Tools & Commands I used whilst studying and during the eJPT exam.
# Recon
There is not much traditonal recon to be done on the exam **BUT** you are provided with a LoE. so READ, READ and READ the LoE! Understand what is being asked of you! and what is the scope
### wireshark
Understanding the outputs from a .pcap file will save you heaps of time. Having a basic understanding of packet filtering and identifying the source and destination of traffic will be beneficial

Some commands that help with the analysis of the capture.
```
ip.add == 10.10.10.10
ip.dest == 10.10.10.10
ip.src == 10.10.10.10
eth.dst == ff:ff:ff:ff:ff:ff
```
# Enumaration 
Usuful common ports

![image](https://user-images.githubusercontent.com/112498030/204086338-c4215d56-b905-4b2a-8b07-da8f3598a5b2.png)

Ping sweep from TCM Practical Ethical Hacking course
```bash
#!/bin/bash
if [ "$1" == "" ] 
then 
echo "You forgot an IP address!" 
echo "Syntax: ./ipsweep.sh 10.10.10" 

else 
for ip in `seq 1 254`; do 
ping -c 1 $1.$ip | grep "64 bytes" | cut -d " " -f 4 | tr -d ":" & 
done 
fi
```
Nmap scanning and OS fingerprinting. -T4 could be used but because the exams allows for 72hrs, I used the time to play around 
```
nmap -Pn --open 10.10.10.10 
nmap -sS -n 10.10.10.0/24
nmap -Pn -A -O 10.10.10.10 
nmap -sC -sV -A -T3 -p- 10.10.10.10 --open
nmap -sU -sV 10.10.10.10
```
massscan  
```
sudo masscan -iL hosts.list -p0-65535 --rate 64000 --open-only
```
Vuln Scanning
Wild recommend using Nessus or the nmap NSE
```
sudo nmap -sV --script=vuln
```

## Web App enum
Server fingerprinting 
```
Nc target.site 80
HEAD / HTTP/ 1.0
```
for HTTPS 
```
Openssl s_client - connect target.site:443 
HEAD / HTTP/1.0
```
for signature based.
```
Httprint -P0 -h <target hosts> -s <signaturefile>
```
>-P0 avoids ping request (web servers do not respond to ping echo requests)

>-h <target hosts> finger print a list of hosts, it Is advised to use IP addresses
  
>-s set the signature file to use

### Directory enum
```
dirb http://10.10.10.10/ /usr/share/dirb/wordlists/common.txt
gobuster -u 10.10.10.10 -w /path/to/wordlist.txt
```
  
# Vulnerability Assessment
**Understanding routing is super important!**. equally is not that complicated beyond adding ip routes 
```
  ip route add 20.20.20.0/24 via 10.10.10.10(VPN Gateway)
```
### Share Enum
```
Enum4linux -P -a -S 10.10.10.10
Enum4linux -s /usr/share/enum4linuz/share-list.txt
```
```
nmblookup -A 10.10.10.10
```
```
smbclient -L //10.10.10.10 -N
```
# Exploitation
  
### Cross Site Scripting aka XSS
1. find a reflection point & test for input
  
```
<h1>Test</h1>
<script> alert('3l1t3');</script>
```  
### SQL
Same as the XSS check all input parameters. GET, POST, HTTP HEADERS ( user-agent, cookie, accept…) test all user inputs!
```
Sqlmap -u 'http: //vulnerable.site/view.php/id=xxx' -p id
Sqlmap -u http: //vulnerable.site/view.php/id=xxx -b -tables
```
useful flags for sqlmap
```
--threat=10 (speed at 10)
--risk=3 (risk of being loud - the higher the risk the louder)
--dbs = database
--dbms= (type of the dbs i.e. mysql)
-D database = dbs name
--tables = looks for tables in the dbs
-T tablename = table
--dump
--level=3 (how aggressive)
--technique=U
-v3 = shows payload used
--flush-session = deletes the information gather on that particular targ-
```
### Metasploit & Meterpreter 
```
Show (name of exploit) = list the path to the exploit
use (name of exploit) = this will enable the exploit
back = will exit from the exploit and back to the msfconsole
info = shows the info on the exploit ( once the exploit is set)
show options = list the exploit options that can be set
set (name of option) information_needed = this configures the option
```
on a meterpreter session using kiwi (mimikatz) to dump password hashes
```
load kiwi
```
```
hashdump
```
### Password cracking & brute forcing
	
John the ripper
```
sudo john -wordlist=/usr/share/wordlists/rockyou.txt Hash.txt
```
Hydra for brute forcing services 
```
hydra -L users.txt -P pass.txt 10 10.10.10.10 (service ssh,ftp) -p (if running on a non-standard port)
```
# Reporting & Final Thoughts
The exam is not a CTF, and not every single machine needs to be rooted! overall it was a fun exam and the free training provided by INE is more than sufficient.
However I do recommend the video services by [OvergrownCarrot](https://www.youtube.com/watch?v=sWHp0WWHwXE&list=PLfWV6Qh-wJ5MDdpGIMhbokwcC5_dLclSP) is defiently worth watching! 
