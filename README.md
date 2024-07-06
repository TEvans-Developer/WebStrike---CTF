# WebStrike---CTF
<i>WebStrike</i> is a CTF created by CyberDefender that gives practical hands on experience learning <b>Network Forensic</b>, utilizing tools like <b>WireShark<b>, <b>PCAP</b> analysis and more... 
<h1>WebStrike CTF</h1>
<h3>Scenario:</h3>
<br> An anomaly was discovered within our company's intranet as our Development team found an unusual file on one of our web servers. Suspecting potential malicious activity, the network team has prepared a pcap file with critical network traffic for analysis for the security team, and you have been tasked with analyzing the pcap.

<br>Tools:

<b>-WireShark (PCAP analysis)</b>

<b>-Kali Linux (VM)</b>
<hr>
<h3> Question 1:</h3>
<i> Understanding the geographical origin of the attack aids in geo-blocking measures and threat intelligence analysis. What city did the attack originate from? </i>
<h3>Approach:</h3>
<br>We are asked to find the geographical orgin of the threat actor(s). We first must use the items provided in our WireShark tool to identify any anomalies such as large amount of packets traversing our network to an IP address. An IP address is a public base, logical address assigned to a device in a location that allows threat actors' to be routed to the internet. Identifying the threat actors IP address will help aid us in finding the region they are from and help validate that this is our threat actor.

<h3>Steps</h3>
<b>Step 1:</b>
<br> After setting up our VM in Kali Linux, download the PCAP file, unzip it using the password code provided and open the file in WireShark. 
<b>Step 2:</b>
<br>Upon opening the file we can observe that there are 355 packets. We also observe in our packet list that there is an <b>Empemeral Port</b>*, <i>43848</i> assigned to an IP address <i>117.11.88.124</i> as the source IP. We want to find unusual amounts of packets being sent to and from different IP addresses. We will navigate to the <b>Statistics</b> tab at the top of WireShark, then navigate to the <b>Conversation</b> tab. Once we are here we will want to navigate to the IPv4 tab and analyize the <b>Address A</b> column and see that out of all our 355 packets in our PCAP file all 355 are being sent either to or from the IP address we identified in our packet list <i>117.11.88.124</i> validating that this is our threat actors IP address.

![WebStrickThreatActorIP](https://github.com/TEvans-Developer/WebStrike---CTF/assets/140648793/1bc10869-bccc-44a3-b385-291d56444bf0)


<b>Step 3:</b>
<br>We will then take our threat actors IP address and use an open source tool such as https://www.whatismyip.com/ip-whois-lookup/ to find the region of the threat actor. 
![WebStrickThreatActorLocation](https://github.com/TEvans-Developer/WebStrike---CTF/assets/140648793/4a64f90a-cd03-43d7-b743-3b320df824f2)

<br><b>Answer:</b><i> Tianjin</i>
<hr>

<h3>Question 2:</h3>
<i>Knowing the attacker's user-agent assists in creating robust filtering rules. What's the attacker's user agent?</i>
<h3>Approach:</h3>
<br>We want to gain information about the threat actors <b>User-Agent</b>. Being able to collect this information can help inform us about the type of host operating system, software name; version, details about the device and its capabilites. Understanding that WireShark provide a tool to follow HTTP Stream to analyize the tcp stream going from the user to the server and back. 
<h3>Steps</h3>

<b>Step 1:</b>
<br>We need to find a packet that has our threat actors IP address as the source making a HTTP type request from our server. ![WebStrickThreatActorUserAgent2](https://github.com/TEvans-Developer/WebStrike---CTF/assets/140648793/3adbde14-044e-4c0d-9a6d-e1a067091db7)



<b>Step 2:</b>
<br>We will then click that packet and navigate down to hover over the <b>Follow</b> option. From here we will click on the <b>HTTP Stream</b> option.


<b>Step 3:</b>
<br>Once we are navigated to the HTTP Stream we will be able to see req. and res. made between our threat actor (red) and our server (blue). We analyize that the threat agent made a <b>"GET"</b> req. (top) to the server on User-Agent:<i>Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0</i>.

![WebStrickThreatActorUserAgent](https://github.com/TEvans-Developer/WebStrike---CTF/assets/140648793/e1155160-d407-48e3-b64d-e60e1379e26e)


<br><b>Answer:</b><i> Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0</i>






<hr>
<h3>Glossery*</h3>
<b>Empemral port</b> - A temporary port assigned to an endpoint to establish connection with a server application over the internet or LAN.
