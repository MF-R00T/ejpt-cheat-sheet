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
# Footprinting & Scanning 
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




# Vulnerability Assessment

# Exploitation

# Reporting & Final Thoughts
