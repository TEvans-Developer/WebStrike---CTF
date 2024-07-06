# WebStrike-CTF
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
<br>Upon opening the file we can observe that there are 355 packets. We also observe in our packet list that there is an <b>*Ephemeral Port</b>, <i>43848</i> assigned to an IP address <i>117.11.88.124</i> as the source IP. We want to find unusual amounts of packets being sent to and from different IP addresses. We will navigate to the <b>Statistics</b> tab at the top of WireShark, then navigate to the <b>Conversation</b> tab. Once we are here we will want to navigate to the IPv4 tab and analyize the <b>Address A</b> column and see that out of all our 355 packets in our PCAP file all 355 are being sent either to or from the IP address we identified in our packet list <i>117.11.88.124</i> validating that this is our threat actors IP address.

![WebStrickThreatActorIP](https://github.com/TEvans-Developer/WebStrike---CTF/assets/140648793/1bc10869-bccc-44a3-b385-291d56444bf0)


<b>Step 3:</b>
<br>We will then take our threat actors IP address and use an open source tool such as https://www.whatismyip.com/ip-whois-lookup/ to find the region of the threat actor. 
![WebStrickThreatActorLocation](https://github.com/TEvans-Developer/WebStrike---CTF/assets/140648793/4a64f90a-cd03-43d7-b743-3b320df824f2)

<br><b>Answer:</b><i> Tianjin</i>
<hr>

<h3>Question 2:</h3>
<i>Knowing the attacker's user-agent assists in creating robust filtering rules. What's the attacker's user agent?</i>
<h3>Approach:</h3>
<br>We want to gain information about the threat actors <b>*User-Agent</b>. Being able to collect this information can help inform us about the type of host operating system, software name; version, details about the device and its capabilites. Understanding that WireShark provide a tool to follow HTTP Stream to analyize the tcp stream going from the user to the server and back. 
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

<h3>Question 3:</h3>
<i>We need to identify if there were potential vulnerabilities exploited. What's the name of the malicious web shell uploaded?</i>
<h3>Approach:</h3>

We want to find if potential vulnerabilities are exploited and to find any malicious data uploaded into the server onces the threat actor gained access. Understanding that WireShark provides filtering options and the ability to follow specific HTTP Stream per packet we can better pin point and find the malicious upload. Knowing different HTTP methods such as *POST, PUT, *GET and DELETE will also help. 

<h3>Steps</h3>

<b>Step 1:</b>
<br> We want to narrow our search to make it easier to find the malicious file that was uploaded to the system. We should input into our display filter <i>ip.src == 117.11.88.124 && http.request.method == POST</i>. This filter is telling us that we want WireShark to find a packet where the source IP is 117.11.88.124 (Threat actor) and (&&) where they made an HTTP req. using the "*POST" method. To futher narrow our search we will navigate to the Edit tab then click "Find Packet". We will then enter "upload" into the filter as a string to be found in our "Packet Details". 

<b> Step 2:</b>
<br> Two packet will appear we want to analyize them until we are able to find an upload made by our threat actor. We will right click the packet and follow the HTTP Stream. After analysis we find that in packet 63 our threat actor upload a file <i>image.jpg.php</i>.
![WebStrickThreatActorFileUpload](https://github.com/TEvans-Developer/WebStrike---CTF/assets/140648793/cfe83f6e-191e-4e88-9787-c3ca36979b01)

<b>Answer: <b><i>image.jpg.php</i>

<hr>
<h3>Question 4:</h3>
<i>Knowing the directory where files uploaded are stored is important for reinforcing defenses against unauthorized access. Which directory is used by the website to store the uploaded files?</i>

<h3>Approach:</h3>
We want to find the senestive data (directory) that the threat actor attempted to infiltrate. Using the filtering options WireShark provides us and understanding HTTP methods we are able to filter through our packet list to find what directory the threat actor attempted to infiltrate.

<h3>Steps</h3>
<b>Step 1:</b>
<br> We first want to understand that the "GET" method is a req. method that allows the retrieval of information from a server as well as where the threat actor is wanting to get this req. from, which is our server. We will input this filter <i>"ip.dst == 24.49.63.79 && http.request.method == GET"</i>. 


<br><b>Step 2:</b>
<br> We then will right- click the packet of interest (Packet 138) then follow the HTTP Stream to see that the threat actor made a "GET" req. to our server to access the <i>/reviews/uploads/</i> od our server.
![WebStrickThreatActorGetReq](https://github.com/TEvans-Developer/WebStrike---CTF/assets/140648793/1af4aeda-091b-46bb-ae6a-ef5d414f0d0c)

<b>Answer:</b><i> /reviews/uploads/</i>

<hr>








<hr>
<h3>Glossery</h3>
<b>*Ephemral Port</b> - A temporary port assigned to an endpoint to establish connection with a server application over the internet or LAN.

<br><b>*GET</b> - is a HTTP method used by clients to retrieve data from a server. This method can be used to gain access to server assests like directories, files, functionalities and more.

<br><b>*POST</b> - is a HTTP method used by clients to send data to a server. This method can be used to submit data or file upload. 

<br><b>*User-Agent</b> - is a string that a web browser or other client software sends to a web sever to identify itself. Helps understand what kind of device or software is accessing the sit so they can deliver the appropriate content. 

 
