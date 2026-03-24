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
Phase 1: High Level Triage <br/>
<img src="https://github.com/CRYPTO-Isaac/WiresharkLab/blob/main/1774365086013-c0c7ac37-c01d-4706-a6e1-9d4d2c5ef5c1.png?raw=true" height="80%" width="80%" alt="Wireshark Steps" />

  The protocol hierachy outlines the types of protools which were mostly used in the timeframe 
  Wireshark - Protocol Hierachy Statistics was utilised
  
  IPv4 and TCP where the main protocols used suggesting the majority of traffic sent and received was over a Wi-Fi connection.
  
Phase 2: Filtering Conversations: What IP addresses talk the most? 
<br />
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

Phase 3: Attack Vector Diagnosis  <br/>
<img src="https://i.imgur.com/cdFHBiU.png" height="80%" width="80%" alt="Wireshark Steps"/>
<br />
<br />
Wait for process to complete (may take some time):  <br/>
<img src="https://i.imgur.com/JL945Ga.png" height="80%" width="80%" alt="Wireshark Steps"/>
<br />
<br />
Sanitization complete:  <br/>
<img src="https://i.imgur.com/K71yaM2.png" height="80%" width="80%" alt="Wireshark Steps"/>
<br />
<br />
Observe the wiped disk:  <br/>
<img src="https://i.imgur.com/AeZkvFQ.png" height="80%" width="80%" alt="Wireshark Steps"/>
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
