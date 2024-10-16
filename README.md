# Credential-Harvesting-Via-Site-Cloning-And-MITM

## 1. Project Overview

Project Title: Redirecting Victim to Cloned Website Using Social Engineering Toolkit (SET) and Ettercap for MITM with ARP Spoofing

Objective: The primary goal of this project is to demonstrate the use of Social Engineering Toolkit (SET) for cloning a website and redirecting a victim's browser to the cloned site using Ettercap for MITM (Man-in-the-Middle) attack with ARP Spoofing. This setup can be used for credential harvesting or understanding the impact of social engineering attacks in a controlled environment.


## 2. Skills Learned


- Network Security & MITM Attacks: Gained hands-on experience with ARP spoofing, DNS spoofing, and executing Man-in-the-Middle (MITM) attacks to manipulate and intercept network traffic.
- Social Engineering & Phishing: Mastered credential harvesting by cloning legitimate websites using the Social Engineering Toolkit (SET) and creating realistic phishing simulations.
- Penetration Testing Tools: Acquired proficiency in tools like SET and Ettercap, and learned how to execute network reconnaissance and vulnerability testing.
- Linux & Network Configuration: Strengthened Linux command-line skills, including IP forwarding, network configuration, and system administration on Kali Linux.
- Protocol Analysis & Security: Developed a solid understanding of network protocols (ARP, DNS, HTTP/HTTPS) and how to exploit their vulnerabilities for ethical hacking.
- Security Awareness & Defense: Enhanced knowledge of security mitigation techniques to defend against ARP and DNS spoofing, emphasizing the importance of encryption and secure network practices.

## 3. Tools and Technologies Used


- Social Engineering Toolkit (SET): A powerful framework for executing social engineering attacks, including credential harvesting using site cloning.
- Ettercap: A comprehensive suite for man-in-the-middle attacks on LAN. It supports active and passive dissection of many protocols and includes ARP poisoning for traffic redirection.
- Kali Linux: A Linux distribution designed for penetration testing, housing a variety of tools including SET and Ettercap.
- VMware: For virtualizing both the attacker (Kali Linux) and victim (Windows 11) environments.
- Victim Machine (Windows 11): The machine on which we will test the redirection to the cloned site.



## 4. Project Setup and Prerequisites
Before diving into the project execution, ensure the following prerequisites are in place:

- Kali Linux is installed and configured on the attacker machine.
- Windows 11 is installed and configured as the victim machine, both running on the same network (e.g., using VMware).
- IP Forwarding is enabled on Kali Linux to allow traffic to pass between the victim and the gateway.
- The Social Engineering Toolkit (SET) is installed on Kali Linux.
- Ettercap is installed and configured on Kali Linux.

## 5. Steps for Project Execution
### 5.1. Cloning a Website Using Social Engineering Toolkit (SET)
Objective: To clone a legitimate website and set up a server that will host the cloned page for credential harvesting.

1. Launch SET: Open a terminal in Kali Linux and start SET by running the following command:
```
sudo setoolkit
```
2. Select the Social Engineering Attack:
- From the main menu of SET, choose 1) Social-Engineering Attacks.
  <img width="1440" alt="Screenshot 2024-10-16 at 4 19 49 PM" src="https://github.com/user-attachments/assets/3c764513-bd96-47bf-9fd9-f9595df4d4f1">

3. Choose Website Attack Vectors:
- Next, choose 2) Website Attack Vectors.
  <img width="1440" alt="Screenshot 2024-10-16 at 4 19 56 PM" src="https://github.com/user-attachments/assets/982c86d8-929e-476b-b1af-1ce41f6680a2">

  
4. Credential Harvester Attack Method:
- Select 3) Credential Harvester Attack Method. This method captures user credentials input on the cloned site.
  <img width="1440" alt="Screenshot 2024-10-16 at 4 20 03 PM" src="https://github.com/user-attachments/assets/12507e8a-6f3f-4e09-9f55-700d9de08075">

  
5. Site Cloning:
- Choose 2) Site Cloner. This option allows you to clone any website of your choice.
  <img width="1440" alt="Screenshot 2024-10-16 at 4 21 35 PM" src="https://github.com/user-attachments/assets/4ece350b-0106-4cc2-840d-d3125f119078">


6. Enter the IP Address:
- You will be prompted to enter the IP address for the cloned website. Enter the IP address of your Kali Linux machine (where the site will be hosted). It is 192.168.2.128

  
7. Enter the Target Website:
- Enter the URL of the website you wish to clone. SET will clone the page and host it on your Kali Linux machine. Here ,we will clone linkedin.
  <img width="1440" alt="Screenshot 2024-10-16 at 4 24 28 PM" src="https://github.com/user-attachments/assets/fac4f338-33d2-466f-ad21-2f9f7357f0fc">
  <img width="1440" alt="Screenshot 2024-10-16 at 4 24 54 PM" src="https://github.com/user-attachments/assets/2be475ba-b75d-4a0e-a21f-b5e3f45ab278">


8. Cloning Successful:
- After SET successfully clones the website, it will start an HTTP server to serve the cloned site on your IP address (e.g., http://192.168.2.128).
  <img width="1440" alt="Screenshot 2024-10-16 at 4 25 02 PM" src="https://github.com/user-attachments/assets/f4b0155f-db06-4ca3-bc80-e73fd231c14b">
  <img width="1440" alt="Screenshot 2024-10-16 at 4 39 26 PM" src="https://github.com/user-attachments/assets/c234f0aa-ed15-4809-b37e-01eaec3689e0">



### 5.2. Man-in-the-Middle (MITM) Setup Using Ettercap and ARP Spoofing
Objective: To intercept and redirect traffic from the victim’s machine to the cloned site using ARP spoofing.

Step 1: Enable IP Forwarding
- To ensure the victim's traffic is routed correctly, enable IP forwarding on your Kali Linux machine:
```
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward
```
- The output should be 1, indicating that IP forwarding is enabled.

Step 2: Identify Victim and Gateway IPs
- Use ifconfig to get your own IP address and the network configuration. Then, use the following command to identify the gateway (router’s) IP:
```
ip route | grep default
```
Use Nmap to identify the victim’s IP address:
```
nmap -sn 192.168.2.0/24
```
From the results, identify the victim’s IP address (Windows 11 machine i.e. 192.168.2.133)

Step 3: Edit the Ettercap DNS Spoofing Configuration File
- Ettercap uses a DNS configuration file to map domain names to IP addresses for DNS spoofing. You will modify this file to redirect a specific domain (e.g., www.Linkedin.com) to the IP address of your cloned site.

Open the etter.dns configuration file for editing:
```
sudo nano /etc/ettercap/etter.dns
```
Add an entry for the domain you want to spoof, redirecting it to your cloned site’s IP. For example, if you cloned Linkedin.com and the cloned site is hosted at 192.168.1.128, add:
```
www.Linkedin.com A 192.168.2.128
```
<img width="1440" alt="Screenshot 2024-10-16 at 6 47 35 PM" src="https://github.com/user-attachments/assets/1d6c78d6-1113-4557-8004-f16a1d4a981e">

Save the file and exit (Ctrl + X, then Y to save).

Step 4: Launch Ettercap in Graphical Mode
- Start Ettercap in graphical mode by running:
```
sudo ettercap -G
```
Step 5: Start Unified Sniffing
- In the Ettercap GUI, go to Sniff → Unified sniffing.
- Select the network interface (eth0 ) through which your Kali machine is connected to the same network as the victim.

Step 6: Scan for Hosts
- Go to Hosts → Scan for hosts to scan the network and list all available devices.


Step 7: Add Victim and Gateway to Target
- In the host list, find the IP address of the victim (Target 1) and the gateway (Target 2).
- Select the victim and the gateway, and add them as Target 1 and Target 2 respectively.
  <img width="1440" alt="Screenshot 2024-10-16 at 5 27 37 PM" src="https://github.com/user-attachments/assets/d127dcad-0163-4c6b-9caf-cf7011876687">


Step 8: Enable ARP Poisoning
- Go to Mitm → ARP poisoning.
- Check the box for Sniff remote connections, then click OK.
  <img width="1440" alt="Screenshot 2024-10-16 at 5 28 38 PM" src="https://github.com/user-attachments/assets/c597a0e7-a4ef-41c1-88aa-3e52f90276e7">
  <img width="1440" alt="Screenshot 2024-10-16 at 5 28 44 PM" src="https://github.com/user-attachments/assets/f47d0ce1-2ce3-4790-b9f3-00071f1fa31f">
  <img width="1440" alt="Screenshot 2024-10-16 at 5 29 13 PM" src="https://github.com/user-attachments/assets/4d307af9-830b-43b5-bbc1-88fe1080bcc6">



Step 9: DNS Spoofing
- To redirect the victim’s DNS queries to your cloned site, use Ettercap’s dns_spoof plugin.
- Go to Plugins → Manage Plugins.
- Scroll down to dns_spoof and click Activate.
  <img width="1440" alt="Screenshot 2024-10-16 at 5 30 19 PM" src="https://github.com/user-attachments/assets/fb2f0607-9ce9-422e-9c89-007799226aae">


### 5.3. Redirecting the Victim to the Cloned Site
With DNS spoofing enabled, when the victim tries to access the legitimate website (e.g., www.linkedin.com), their traffic will be redirected to the cloned version hosted on your Kali Linux machine.

Testing the Setup:
1. On the Victim's Machine:
- Open a browser on the victim’s Windows 11 machine.
- Enter the URL of the legitimate site you cloned (e.g., http://www.Linkedin.com).
  
2. Redirection:
- The browser will load the cloned website served by SET on the attacker's machine (e.g., http://192.168.2.128), making it appear as if the user is accessing the legitimate site.

3. Credential Harvesting:
- Any credentials entered by the victim on the cloned site will be captured by the Social Engineering Toolkit and displayed in your Kali terminal where SET is running.
  <img width="1440" alt="Screenshot 2024-10-16 at 7 06 24 PM" src="https://github.com/user-attachments/assets/6762f28d-c877-4072-83d3-aa2f007e4f8a">
  <img width="1440" alt="Screenshot 2024-10-16 at 7 09 26 PM" src="https://github.com/user-attachments/assets/29ac03f1-6bad-4925-a377-be07fc25f2ba">


## 6. Post-Attack Clean-Up
Disabling IP Forwarding:
- Once the attack demonstration is complete, disable IP forwarding to restore normal network traffic:
```
echo 0 | sudo tee /proc/sys/net/ipv4/ip_forward
```
 <img width="1440" alt="Screenshot 2024-10-16 at 7 23 03 PM" src="https://github.com/user-attachments/assets/cd954e2e-dcac-4893-8d1e-0a07d507520d">

Stopping ARP Poisoning:
- In Ettercap, go to Mitm → Stop mitm attack to stop ARP poisoning.
  <img width="1440" alt="Screenshot 2024-10-16 at 7 25 41 PM" src="https://github.com/user-attachments/assets/aa5e10f3-ca8f-49a8-a989-0a675bbee4b5">
  <img width="1440" alt="Screenshot 2024-10-16 at 7 25 59 PM" src="https://github.com/user-attachments/assets/af2eb486-1b99-424c-b186-e766b859e237">

Flushing ARP Cache:
- On the victim machine (Windows 11), flush the ARP cache to remove the spoofed ARP entries:
```
arp -d *
```

## 7. Security Implications
This project highlights the dangers of ARP spoofing and DNS spoofing in a local area network. It showcases how attackers can perform a man-in-the-middle attack to redirect users to phishing sites, making it difficult for victims to distinguish between legitimate and cloned websites. These methods are commonly used in penetration testing to assess the security posture of an organization and demonstrate the importance of secure network configurations and defenses against MITM attacks.

## 8. Conclusion
This project demonstrates a complete attack lifecycle involving the Social Engineering Toolkit for website cloning and Ettercap for MITM with ARP Spoofing. The attack shows how vulnerable users on the same network can be redirected to a cloned site to capture sensitive information like login credentials. This project highlights the importance of network security measures, such as encrypted communication protocols, ARP protection, and user awareness, in mitigating social engineering and man-in-the-middle attacks.

