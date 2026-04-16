# Final-Project-BFOR643
Showcase of our final project presenting a dive into NIST's Incident Response Framework with demonstration using Wireshark and Altoro Mutual.

# Table of Contents

1. [Project Introduction](#Project-Introduction)
2. [Project Relevance](#Project-Relevance)
3. [Methodology](#Methodology)
4. [Attack Vector: XSS](#Attack-Vector-XSS)
   -  [PCAP Dive](#PCAP-Dive)
   -  [Minimizing Attack Surface](#Minimizing-Attack-Surface)
5. [Attack Vector: Session Hijacking](#Attack-Vector-Session-Hijacking)
6. [Atack Vector: HTTP/S](#Attack-Vector-HTTPS)
7. [Conclusion](#Conclusion)
8. [Sources](Sources)


## Project Introduction
We will be using a PCAP analysis of Altoro Mutual. Altoro Mutual is a fictitious banking website developed by IBM. The website uses HTTP for the purpose of demonstrating vulnerabilities and sharpening incident response reactions and techniques. For the purpose of this project, we will be going over protocols, identifying attack vectors, implementing incident response lessons, and taking steps to minimize our attack surface.  

<br>

## Project Relevance
<img width="306" height="126" alt="image" src="https://github.com/user-attachments/assets/6a06571a-ae03-46f8-94d9-846eb4ccf633" />

<br><br>

Incident response is a continuous, auditable, and fluid life cycle aligned with risk management, compliance, and governance. The purpose of maintaining an incident response team and guidebook is to mitigate the risks associated with computer security compromise.

<br>

The previous incident response life cycle model that was presented by NIST in 2012 seemed to lack depth and flexibility as incidents weren’t very common when it was created. According to the publication, the response and recovery was typically completed within two days and incidents were mostly specific to the environment.

<br><br>

<img width="356" height="225" alt="image" src="https://github.com/user-attachments/assets/0c36eb2c-a326-4216-afe0-628a444c8964" />

<br><br>

The current incident response life cycle model by NIST from 2025 in Revision 3 of the same publication has evolved as the modern attack surface increases and brings about more devastating, overwhelming, and persistent breaches. The high-level overview of the model below include 6 functions each with distinctive roles. For the scope of this project and presentation, we will be focusing on the Identify, Detect, and Respond functions where we aim to understand an organizations attack surface, analyze the compromises found, and take actions to mitigate and harden weak points. 

<br><br>

<img width="257" height="216" alt="image" src="https://github.com/user-attachments/assets/9c408338-5a60-4f7e-8ab9-c9a095e52204" />


## Methodology

Wireshark is an open source tool used to capture network traffic passively. The tool is free to use and very simple to navigate. For the purposes of this project, I have selected to listen on my ethernet connection, open up Altoro Mutual on my web browser, inject my code into input fields on the web application and anything else I needed to include in the project, and stopped the capture. Next comes the filtering which was straight forward and includes helpful prompt completion as seen in the Images

<br><br>

*Available network adapters for listening*
<img width="1205" height="440" alt="image" src="https://github.com/user-attachments/assets/3fef282c-15f9-463e-9c52-7bae87af2c2b" />

<br><br>

*Filtering by Ip address*
<img width="975" height="300" alt="image" src="https://github.com/user-attachments/assets/a8b1c4d5-4efc-44cb-8e7a-45ed6f16e346" />

<br><br>

Breaking down the actual PCAP we have the packet list pane in the top showing the number of the packet, time, source, destination, protocol, length, and a quick summary. The left pane shows the entire packet partitioned by OSI layer and then the hex dump and ASCII in the right pane.

<br><br>

<img width="1572" height="921" alt="image" src="https://github.com/user-attachments/assets/675ba00f-f736-495c-a8d1-863c216701df" />

<br><br>

## Attack Vector: XSS

Cross-Site Scripting (XSS) is a type of injection attack where malicious scripts are inserted into otherwise legitimate and trusted websites. It happens when a web application accepts user input and includes it in its output without properly validating or encoding it. This allows an attacker to send harmful code to other users, which their browsers then execute as if it came from a trusted source.

<br>

Looking to the image below, we see a short and simple HTML injected script ​

<br><br>

*Injected script 1*

```
<script>alert('XSS')</script>
```

<img width="780" height="381" alt="image" src="https://github.com/user-attachments/assets/3f912862-19ea-46f5-b89d-d4714a564999" />

​<br><br>

This inputted text of HTML forces the website to execute and have a pop-up box show for the user on your screen.

<br>

Looking at the image below, we see a script with a clickable link that populates on the users page resulting in them getting rick-rolled.​

​
*Injected script 2*

```
<a href="https://tinyurl.com/bddvm824">Click me</a>
```

<img width="776" height="604" alt="image" src="https://github.com/user-attachments/assets/176c86e6-1df9-4f64-8928-a0b5f6917a42" />

<br><br>

There are others ways to perform this attack but the general goal is to get malicious code to run on another users browser by tricking the browser into accepting injected code. This can also result in stolen cookies, hijacked sessions, and modifying the webpage. 
<br>
### PCAP Dive

Looking at the PCAP capture (image 1), you can see that there are two packets sent by my machine and two sent from Altoro. In the short snippet in the “Info” column of packets 84 and 30675, you can see the code snippets from  the earlier screenshots. Obviously, you wouldn’t see that if this was over HTTPS because of the encryption, but that’s exactly what Altoro was created for.

*Image 1*

<img width="1246" height="187" alt="image" src="https://github.com/user-attachments/assets/074c5ed7-6c3c-4604-9931-e94d8c789c28" />


As we dive deeper into Packet 84, we see in the HTTP space of the packet that we have the injected code in plaintext and the Cookie for my unique session.


*Packet 84*​

<img width="1241" height="736" alt="image" src="https://github.com/user-attachments/assets/131588cd-ca04-4d07-8b2f-f9d2e9aa3e0a" />

​





This gets more complicated once you obtain a different users session ID, but this would be very difficult if the website uses HTTPS and more so if it sanitizes user input.​

​

Most applications built with modern frameworks such as React, Angular, and Polymer have fewer vulnerable avenues for XSS to exist in but ensure security measures such as variable validation and sanitization. Validation and sanitization include restraining user input to expected formats for the input fields such as only numbers or letters and restricting special characters and converting them into safe representations so that the browser does not interpret them as HTML, JavaScript or any executable piece.​


### Minimizing Attack Surface

*Angular, React, Polymer*

<img width="782" height="187" alt="image" src="https://github.com/user-attachments/assets/9f593ce1-f9d0-4f72-86b2-3477d6881f24" />


Most applications built with modern frameworks and tools such as React, Angular, and Polymer have fewer vulnerable avenues for XSS to exist in but ensure security measures such as variable validation and sanitization. Validation and sanitization include restraining user input to expected formats for the input fields such as only numbers or letters and restricting special characters and converting them into safe representations so that the browser does not interpret them as HTML, JavaScript or any executable piece.​
​

## Attack Vector: Session Hijacking

​Session hijacking is the act of gaining someone’s unique session ID and making the server believe that its yours can make the server treat you as that authenticated user.

Image 1 shows my session ID that alerted onto my screen after injecting the code shown into the search field on Altoro Mutual.

*Image 1*

*Injected Script*

```
<script> alert(document.cookie); </script>
```


<img width="685" height="667" alt="image" src="https://github.com/user-attachments/assets/b50f2f7d-b239-43b7-9d5b-1f9b8d853584" />


​

Image 2 shows a captured JSessionID which is a session cookie used by Java-based web apps.o mitigate this, simply use HTTPS as a default, no reason to not use HTTPS for a public web app unless you want to open yourself up to vulnerability and no severance package from your employer.


*Image 2*

*Injected Script*

```
<script> alert(document.cookie); </script>
```


​<img width="1277" height="1108" alt="image" src="https://github.com/user-attachments/assets/4619b41c-4acf-48bf-885c-2a6c89b9356f" />


## Atack Vector: HTTP/S


HTTPS (Hypertext Transfer Protocol Secure) is an extension of HTTP that operates over Transport Layer Security (TLS), providing confidentiality, integrity, and authenticity for data in transit. During the TLS handshake, the client and server negotiate cryptographic parameters and establish a shared symmetric session key using asymmetric (public key) cryptography. The server presents an X.509 digital certificate, which is validated by the client against trusted Certificate Authorities (CAs) to ensure the server’s identity. Once the handshake is complete, all application-layer data is encrypted using symmetric encryption, ensuring efficient and secure communication. This prevents eavesdropping, man-in-the-middle attacks, and unauthorized data modification, as intercepted traffic cannot be decrypted without access to the negotiated session keys.​

​

Image 1 is an example of a plaintext login through altoro through HTTP

<br>

<img width="1278" height="1391" alt="image" src="https://github.com/user-attachments/assets/ec546cdd-709a-4014-9cc4-851804a47125" />

<br>

The image above shows a filter done on the captuyre looking for any packets with a source or destination address of 65.61.137.117 for Altoro Mutuals public IP address. The result brough me to packet 240 where it shows the username and password for the account of John Smith. The image below is a closer view of the contents.


<br>

​<img width="659" height="427" alt="image" src="https://github.com/user-attachments/assets/0da27ba5-ef47-419a-9d7b-c24dfee82701" />




<br>


## Conclusion

This project demonstrates how modern web application vulnerabilities can be exploited across multiple layers of the attack surface by analyzing three key attack vectors: Cross-Site Scripting (XSS), insecure HTTP communication, and session hijacking. Through the XSS example, the project highlights how improper input validation allows attackers to inject malicious JavaScript, enabling client-side exploitation such as cookie theft. The analysis of HTTP versus HTTPS emphasizes the importance of encrypted communication, showing how the absence of TLS exposes sensitive data, including session identifiers, to interception. Building on this, the session hijacking demonstration illustrates how attackers can leverage stolen session cookies to impersonate authenticated users without needing credentials.

These attack vectors collectively reinforce the importance of a layered security approach aligned with the NIST Incident Response lifecycle, particularly within the Identify, Detect, and Respond phases. By identifying vulnerabilities (XSS, lack of HTTPS), detecting insecure transmissions and malicious activity (via tools like Wireshark), and responding through mitigation strategies (input validation, HTTPS enforcement, secure cookie attributes), the project showcases how organizations can reduce risk and strengthen their security posture. Overall, this work underscores how seemingly small misconfigurations can chain together into critical security failures, resulting in full session compromise and unauthorized access.


## Sources


1. https://owasp.org/www-community/attacks/xss/
2. https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html
3. https://owasp.org/www-community/attacks/Session_hijacking_attack
4. https://nvlpubs.nist.gov/nistpubs/specialpublications/nist.sp.800-61r2.pdf
5. https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r3.pdf
6. https://datatracker.ietf.org/doc/html/rfc2616
7. https://datatracker.ietf.org/doc/html/rfc1945 





