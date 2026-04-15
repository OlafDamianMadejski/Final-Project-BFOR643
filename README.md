# Final-Project-BFOR643
Showcase of our final project presenting a dive into NIST's Incident Response Framework with demonstration using Wireshark and Altoro Mutual.

# Table of Contents

1. [Project Introduction](#Project-Introduction)
2. [Project Relevance](#Project-Relevance)
3. [Methodology](#Methodology)
4. [Attack Vector: XSS](#Attack-Vector:-XSS)


## Project Introduction
We will be using a PCAP analysis of Altoro Mutual. Altoro Mutual is a fictitious banking website developed by IBM. The website uses HTTP for the purpose of demonstrating vulnerabilities and sharpening incident response reactions and techniques. For the purpose of this project, we will be going over protocols, identifying attack vectors, implementing incident response lessons, and taking steps to minimize our attack surface.  

## Project Relevance
<img width="306" height="126" alt="image" src="https://github.com/user-attachments/assets/6a06571a-ae03-46f8-94d9-846eb4ccf633" />

Incident response is a continuous, auditable, and fluid life cycle aligned with risk management, compliance, and governance. The purpose of maintaining an incident response team and guidebook is to mitigate the risks associated with computer security compromise.

The previous incident response life cycle model that was presented by NIST in 2012 seemed to lack depth and flexibility as incidents weren’t very common when it was created. According to the publication, the response and recovery was typically completed within two days and incidents were mostly specific to the environment.

<img width="356" height="225" alt="image" src="https://github.com/user-attachments/assets/0c36eb2c-a326-4216-afe0-628a444c8964" />


The current incident response life cycle model by NIST from 2025 in Revision 3 of the same publication has evolved as the modern attack surface increases and brings about more devastating, overwhelming, and persistent breaches. The high-level overview of the model below include 6 functions each with distinctive roles. For the scope of this project and presentation, we will be focusing on the Identify, Detect, and Respond functions where we aim to understand an organizations attack surface, analyze the compromises found, and take actions to mitigate and harden weak points.  

<img width="257" height="216" alt="image" src="https://github.com/user-attachments/assets/9c408338-5a60-4f7e-8ab9-c9a095e52204" />


## Methodology

Wireshark is an open source tool used to capture network traffic passively. The tool is free to use and very simple to navigate. For the purposes of this project, I have selected to listen on my ethernet connection, open up Altoro Mutual on my web browser, inject my code into input fields on the web application and anything else I needed to include in the project, and stopped the capture. Next comes the filtering which was straight forward and includes helpful prompt completion as seen in the Images


*Available network adapters for listening*
<img width="1205" height="440" alt="image" src="https://github.com/user-attachments/assets/3fef282c-15f9-463e-9c52-7bae87af2c2b" />

*Filtering by Ip address*
<img width="975" height="300" alt="image" src="https://github.com/user-attachments/assets/a8b1c4d5-4efc-44cb-8e7a-45ed6f16e346" />


Breaking down the actual PCAP we have the packet list pane in the top showing the number of the packet, time, source, destination, protocol, length, and a quick summary. The left pane shows the entire packet partitioned by OSI layer and then the hex dump and ASCII in the right pane.

<img width="1572" height="921" alt="image" src="https://github.com/user-attachments/assets/675ba00f-f736-495c-a8d1-863c216701df" />


## Attack Vector: XSS

Cross-Site Scripting (XSS) is a type of injection attack where malicious scripts are inserted into otherwise legitimate and trusted websites. It happens when a web application accepts user input and includes it in its output without properly validating or encoding it. This allows an attacker to send harmful code to other users, which their browsers then execute as if it came from a trusted source.


Looking to the image below, we see a short and simple HTML injected script ​


*Injected script 1 = <script>alert('XSS')</script>*​

<img width="780" height="381" alt="image" src="https://github.com/user-attachments/assets/3f912862-19ea-46f5-b89d-d4714a564999" />

​

This inputted text of HTML forces the website to execute and have a pop-up box show for the user on your screen.

Looking at the image below, we see a script with a clickable link that populates on the users page resulting in them getting rick-rolled.​

​

*Injected script 2 = <a href="https://tinyurl.com/bddvm824">Click me</a>*

<img width="776" height="604" alt="image" src="https://github.com/user-attachments/assets/176c86e6-1df9-4f64-8928-a0b5f6917a42" />

There are others ways to perform this attack but the general goal is to get malicious code to run on another users browser by tricking the browser into accepting injected code. This can also result in stolen cookies, hijacked sessions, and modifying the webpage. 



## Conclusion

## Sources
