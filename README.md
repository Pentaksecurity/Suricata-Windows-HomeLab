# Suricata-Windows-HomeLab

GitHub Project Summary

Repository Name: suricata-windows-homelab

README.md Example:

# Suricata Windows Homelab

This project demonstrates a hands-on Intrusion Detection System (IDS) lab using Suricata on a Windows VM. It includes setup instructions, custom rules, and testing methodology with simulated traffic from Kali Linux.

## Features

- Suricata IDS installed on Windows
- Custom rules for detecting:
  - ICMP ping requests
  - SMB connection attempts (port 445)
  - Arbitrary TCP connections (e.g., port 4444)
- Real-time alert monitoring using `fast.log` and optional `eve.json`
- Safe simulated attacks from a Kali Linux VM

## Setup

1. **Install Suricata on Windows**
   - Download Suricata and Npcap.
   - Ensure Npcap is installed with WinPcap API compatibility.
   
2. **Configure network interface**
   - Find your NIC GUID with PowerShell:
     ```powershell
     Get-NetAdapter | ForEach-Object { "\Device\NPF_$($_.InterfaceGuid)" }
     ```
<img width="1017" height="46" alt="Screen Shot 2025-09-06 at 4 49 09 PM" src="https://github.com/user-attachments/assets/f16ff0df-b403-4186-8312-50529bcda771" />

     
   - Update `suricata.yaml`:
     ```yaml
     pcap:
       - interface: \Device\NPF_{YOUR_GUID_HERE}
     ```
<img width="875" height="194" alt="Screen Shot 2025-09-06 at 4 52 16 PM" src="https://github.com/user-attachments/assets/5f1151be-66b7-4121-9df2-d3e7da3de12b" />


3. **Add custom rules**
   ```suricata
   alert icmp any any -> any any (msg:"ICMP Ping Detected"; sid:1000001; rev:1;)
   alert tcp any any -> any 445 (msg:"Possible SMB connection attempt"; sid:1000003; rev:1;)
   alert tcp any any -> any 4444 (msg:"Connection attempt to port 4444"; sid:1000010; rev:1;)

   <img width="1024" height="768" alt="Rules" src="https://github.com/user-attachments/assets/ab8369cb-05cf-42d5-bfe1-ac96932ba01e" />



Run Suricata

suricata.exe -c suricata.yaml -i "\Device\NPF_{YOUR_GUID}" -s rules\homelab.rules

<img width="1024" height="768" alt="Suricata-Start" src="https://github.com/user-attachments/assets/a3e11d49-c1a9-42d3-be8e-a31fbc0bdefc" />



Test traffic from Kali Linux

Ping the Windows VM to trigger ICMP rules

<img width="551" height="282" alt="Screen Shot 2025-09-06 at 4 44 26 PM" src="https://github.com/user-attachments/assets/2a509c19-ca05-48f0-acf3-9a0b9b39ae66" />


Use smbclient or nc to test SMB (port 445)

<img width="439" height="72" alt="Screen Shot 2025-09-06 at 4 44 53 PM" src="https://github.com/user-attachments/assets/cadd5fc8-5094-4380-9f17-32abfcfd4b60" />

Logs

<img width="1024" height="768" alt="Suricata_Log" src="https://github.com/user-attachments/assets/af120492-9511-4257-bfa4-b55da7d00557" />


Alerts are written to C:\Program Files\Suricata\log\fast.log

Optional JSON logging via eve.json

Learning Outcomes

- Configured a real-time IDS lab environment

- Learned Suricata rule syntax and testing methodologies

- Practiced network traffic generation and monitoring

- Gained insights into Windows network security and alert analysis
