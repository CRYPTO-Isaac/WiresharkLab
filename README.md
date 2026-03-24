# WiresharkLab
Intrusion detection by analysing PCAP files captured in a timeframe of 2 weeks


<h1>Wireshark PCAP analysis: intrustion </h1>

<h2>Description</h2>

In this lab I investigated the network traffic captured on a specfic network. This network was flagged as suspicious from the client and they required further investigatioin and so I forsaw that and documented the findings.

<br />


<h2>Languages and Utilities Used</h2>

- <b>Wireshark</b> 

<h2>Environments Used </h2>

- <b> Windows

<h2>Report walkthrough</h2>

<p align="center">
<h3>Phase 1: High Level Triage</h3> <br/>
<img src="https://github.com/CRYPTO-Isaac/WiresharkLab/blob/main/1774365086013-c0c7ac37-c01d-4706-a6e1-9d4d2c5ef5c1.png?raw=true" height="80%" width="80%" alt="Wireshark Steps" />

  The protocol hierachy outlines the types of protools which were mostly used in the timeframe 
  Wireshark - Protocol Hierachy Statistics was utilised
  
  IPv4 and TCP where the main protocols used suggesting the majority of traffic sent and received was over a Wi-Fi connection.
  
<h3>Phase 2: Filtering Conversations: What IP addresses talk the most? </h3>
<br />

<img src="https://github.com/CRYPTO-Isaac/WiresharkLab/blob/main/Screenshot%202026-03-24%20at%2016.13.16.png?raw=true" height="80%" width="80%" alt="Wireshark Steps"/>
<br />
<br />
Further information about the breakdown of the conversations shows the amount of packets being sent by the devices range from (2 - 15) which is very small counts. In addition to this, the byte size ranged from (100bytes – 1kB). 
<br />
<br />
The pattern suggests port probing/ scanning, not normal user sessions because of how TCP reconnaissance works at a protocol level. In a normal session, multiple events take place within the connection which leads to sustained communication, larger packet exchange and byte count. 
<br />
<br />
However in this instance of a port scan, the only event that takes place is asking, “is the port open” and there is no data exchange and no sustained communication. This confirms the pattern of reconnaissance being performed by the external devices on the internal device.

<br />
<br />

<h3>Phase 3: Attack Vector Diagnosis </h3> <br/>
<img src="https://github.com/CRYPTO-Isaac/WiresharkLab/blob/main/Screenshot%202026-03-24%20at%2016.13.23.png?raw=true" height="80%" width="80%" alt="Wireshark Steps"/>
<br />
The external devices are most likely doing a service enumeration which is a common technique used to identify specific software versions, configurations and applications running on the open network ports which helps finding exploitable vulnerabilities. <br /><br />
 
 Further confirmation is given when we use the filter 
 tcp.flags.syn == 1 && tcp.flags.ack == 0
 Sequential SYN packets sent from 146.90.197.173 was sent to different ports on 192.168.1.200 therefore the external machine is doing a reconnaissance.
 
<br />
<br />

The external machine takes part in banner grabbing as they completed the handshake long enough to confirm the port is open and identify the SSH version running
 - SSH sends banner after handshake (SSH-1.99-OpenSSH_4.4)
 There was no attempts of authentication or command execution observed.
 <br/>
<img src="https://github.com/CRYPTO-Isaac/WiresharkLab/blob/main/Screenshot%202026-03-24%20at%2016.13.32.png?raw=true" height="80%" width="80%" alt="Wireshark Steps"/>
<br />
<br />

<h3>POST requests</h3>
 We applied the filter http.request.method == “POST” to see the POST requests that were present in the network and we found that: 
 <br />
 <br />
 • Multiple POST requests<br />
 • From 146.90.197.173 → 192.168.1.200 <br />
 • Targeting many different PHP endpoints <br />
 • Lengths between ~300–1200 bytes <br /><br /><br />
 This does not look like normal browsing and the targets the POST requests were trying to reach suggested patterns of web browser exploitation and attempts and vulnerability scanning
 The attacker is: <br /><br />
 • Trying multiple CMS paths <br />
 • Testing known vulnerable endpoints<br />
 • Attempting file upload scripts<br />
 • Probing admin panels<br />
 • Targeting WordPress paths<br />
 • Targeting LibreCMS<br />
 • Targeting Joomla components<br />
 • Targeting upload scripts<br />
 • This is classic web exploit kit / automated exploitation scanning.<br />


 <h3> Phase 4: Remidiation actions </h3>
 <br />
 - Disable unnecessary services <br />
- Telnet (insecure → remove immediately) <br />
- FTP (replace with SFTP/FTPS) <br />
- VNC (restrict or remove if not needed) <br /> <br />
🔒 Restrict access to required services <br />
- Use firewall rules: <br />
- Allow SSH only from trusted IPs <br />
- Block all external access by default (deny-all rule)  <br /> <br />
🌐 Network segmentation  <br />
- Move internal services behind NAT/firewall  <br />
- Do NOT expose directly to the internet unless required  <br />





<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
