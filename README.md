<h1>Traffic Analysis Using Wireshark</h1>

<h2>Objective</h2>
The objective of this project was to conduct packet analysis of unencrypted network traffic to demonstrate the vulnerability of plaintext protocols. By capturing and reconstructing sensitive login credentials transmitted using HTTP, this lab serves as a proof-of-concept for the risks of data interception and highlights the need for end-to-end encryption using HTTPS.
<br />

<h2>Project Summary</h2>
This project demonstrates a successful Man-in-the-Middle (MitM) analysis within a controlled virtual environment. By intercepting traffic between a victim workstation and a web server, I successfully identified critical vulnerabilities in unencrypted protocols, including the exposure of plaintext credentials and session data.
<br />

<h2>Languages and Utilities Used</h2>

- <b>PowerShell</b> 
- <b>Wireshark</b>

<h2>Environments Used </h2>

- <b>Ubuntu(Linux)</b>
- <b>VirtualBox</b>
- <b>Windows 11 Host</b>

<h2>Project Report:</h2>

<p align="left">
<h2>Step 1: Default Gateway redirection</h2>
 
- <b>Purpose: To establish a Man-in-the-Middle (MitM) position by reconfiguring the victim's network path to route through the attacker VM(192.168.87.4). This enables transparent interception and packet inspection of all outbound traffic.</b>
<p align="center">
<img width="541" height="58" alt="ws wu1" src="https://github.com/user-attachments/assets/67bfce93-02e5-426a-a8a1-6767b6d492df" />

- <b>Verification of the updatednetwork path: </b>
<p align="center">
<img width="624" height="61" alt="ws wu2" src="https://github.com/user-attachments/assets/e4a1ac06-662c-43dd-bcf2-b4c0cb5c0664" />


<h2>Step 2: Enable Packet Forwarding</h2>
 
- <b>Purpose: Transform the MitM VM from a standard end-host to a functioning router, forwarding the victim’s traffic to the internet while performing transparent interception of the data.</b>
<p align="center">
<img width="481" height="57" alt="ws wu3" src="https://github.com/user-attachments/assets/ada5b6d2-124a-4bfd-8858-92ca5f160dd9" />

- <b>Verification: The return value of “1” tells us that the change was successful. </b>
<p align="center">
<img width="456" height="36" alt="ws wu 4" src="https://github.com/user-attachments/assets/bfcd65af-b5de-4cce-b01a-1cd2e76b75c7" />

<h2>Step 3: DNS Resolution</h2>
 
- <b>Purpose: Bypass local DNS issues (no connection) by pointing the system’s DNS resolver to Google’s DNS to resolve domain names into IP addresses.</b>
<p align="center">
<img width="624" height="55" alt="ws wu 5" src="https://github.com/user-attachments/assets/d982c0f0-8cfd-48fc-92a4-a0901bd50dc4" />


- <b>Verification of connection to Google DNS using ping:</b>
<p align="center">
<img width="565" height="147" alt="ws wu 6" src="https://github.com/user-attachments/assets/a4664d0d-2cc0-46cc-b46e-90b9eaa14cca" />

<h2>Step 4: Network Address Translation and Masquerading</h2>
 
- <b>Purpose: By enabling IP Masquerading on the external interface (Internet facing via enp0s8). It allows the private, non-routable IP addresses of internal victim machines to be translated into the gateway’s public IP address for outbound communication. This not only provides internet access to the private network but also hides the internal network topology from the external.</b>
<p align="center">
<img width="624" height="17" alt="ws1" src="https://github.com/user-attachments/assets/54bff8f1-a5ce-45d4-8482-1b4470f12a40" />
 
- <b>Verification/Analyzing Post-Routing NAT Traffic:</b>
<p align="center">
<img width="624" height="288" alt="ws 2" src="https://github.com/user-attachments/assets/4f51ddbd-8a86-4746-8718-65050d1b1ab0" />
 
- <b>Verification Analysis: The output shows the packet counters for the POSTROUTING chain.</b>
- <b>As seen in the screenshot, the counter shows 524 packets (69,261 bytes) hitting the MASQUERADE rule.</b>
- <b>These active counters prove that the NAT table is successfully translating the victim's private traffic into the gateway's public IP address.</b>

<h2>Step 5: Victim Login</h2>
 
- <b>Purpose: To provide the data to capture in Wireshark, the victim will enter their login credentials to a website utilizing the insecure HTTP protocol.</b>
<p align="center">
<img width="389" height="349" alt="ws3" src="https://github.com/user-attachments/assets/9d03b54c-0fee-4fb8-af0f-0f1884ee9f08" />

<h2>Step 6: Wireshark Analysis</h2>
 
- <b>Purpose: Conduct deep packet inspection and uncover the sensitive credentials from the victim.</b>
- <b>*The POST Request will contain the user credentials as these are designed to submit data.</b>
- <b>*Notice the HTTP protocol</b>
<p align="center">
<img width="624" height="401" alt="ws analysis" src="https://github.com/user-attachments/assets/49cffd0d-e96b-4c05-bc28-f7005829ae14" />
 
- <b>As you can see, the sensitive credentials are displayed in plaintext within the captured packet.</b>
- <b>The credentials captured by the attacker display:</b>
- <b>Username: Login_Test</b>
- <b>Password: CanYouSeeThis</b>

<h2>Step 7: HTTP vs HTTPS Comparison</h2>
 
- <b>To demonstrate how Transport Layer Security (TLS) ensures data confidentiality and protects against credential harvesting, even when the network path is compromised by a Man-in-the-Middle.</b>
- <b>The user will now enter their login through a website where the traffic is encrypted.</b>
<p align="center">
<img width="356" height="289" alt="ws tls" src="https://github.com/user-attachments/assets/4bcdb506-8e28-4791-a91c-7b0a54f1c390" />
 
- <b>The user data is will be transformed into unreadable ciphertext.</b>
- <b>*Notice the TLS protocol</b>
<p align="center">
<img width="623" height="480" alt="ws tls1" src="https://github.com/user-attachments/assets/72aef29b-3d70-4bd4-9dca-01f961590ef4" />

- <b>As you can see, the sensitive credentials are unreadable even though we captured the packet.</b>
- <b>The attacker can see that a connection exists, but the application data is encapsulated within a TLS v1.3 tunnel.</b>

<h2>Final Summary</h2>
The completion of this lab highlights the critical importance of encryption in modern networking. By establishing a MitM position, I demonstrated that any data passing through a compromised gateway is exposed when using plaintext protocols like HTTP.
<br />

<h2>Key Takeaways:</h2>

- <b>Dangers of HTTP:</b> This plaintext protocol provides zero protection for users, allowing sensitive credentials to be captured and reconstructed.
- <b>Effectiveness of HTTPS:</b> Despite successful traffic interception, the data remained unreadable due to TLS encryption. This proves that data confidentiality can be maintained even when the network connection is compromised.
- <b>Defense-in-Depth:</b> Security is a multi-layered responsibility. While enforcing HTTPS is vital, hardening the network infrastructure is equally important to prevent gateway redirection that makes MitM attacks possible.
 
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
