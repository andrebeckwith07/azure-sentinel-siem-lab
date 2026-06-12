# Azure Sentinel SIEM Home Lab – Honeypot & Attack Map

## Overview
Deployed a Windows 10 honeypot VM in Azure, forwarded security logs to Microsoft Sentinel, and built a live attack map to visualize real-world brute-force attacks by geolocation.

**Tools used:** Microsoft Azure, Microsoft Sentinel, Log Analytics Workspace, KQL, Windows 10

---

## Architecture
<img width="700" src="https://github.com/user-attachments/assets/d03272e6-92cf-401a-8286-25aaf8e6ffc5" />

Real-world attackers on the public internet hit the honeypot VM through an intentionally open NSG. The VM forwards logs to a Log Analytics Workspace, which feeds into Microsoft Sentinel. Sentinel powers the live attack map showing where attacks are coming from globally.

---

## Step 1 – Deployed the Honeypot VM
<img width="700" src="https://github.com/user-attachments/assets/bd9f017e-5a02-4f82-be6d-5b419d0728aa" />

Deployed a Windows 10 VM in Azure configured as a honeypot. Set the Network Security Group to allow all inbound traffic to expose it to the internet and attract real attackers.

---

## Step 2 – Observed Failed Login Attempts in Event Viewer
<img width="700" src="https://github.com/user-attachments/assets/5127615c-8484-4f68-97e0-9d38e155f618" />

RDP'd into the honeypot VM and opened Event Viewer. Found 36,000+ Event ID 4625 audit failures. These are real attackers brute-forcing their way in using common usernames like ADMIN, REMOTE, and TEST.

---

## Step 3 – Queried Failed Logins with KQL
<img width="700" src="https://github.com/user-attachments/assets/0eda64f6-d23f-41f9-aaed-bb71bd227aac" />

Forwarded security logs to a Log Analytics Workspace and used KQL to query Event ID 4625. Results show attacker IP addresses, account names, and timestamps of every failed login attempt.

---

## Step 4 – Connected Microsoft Sentinel as the SIEM
<img width="700" src="https://github.com/user-attachments/assets/2023442f-297b-4ece-95ec-0018ec5aec09" />

Connected Microsoft Sentinel and configured the Windows Security Events via AMA data connector. The dashboard shows 195K security events collected and logs being received in real time.

---

## Step 5 – Enriched Logs with Geolocation Data
<img width="700" src="https://github.com/user-attachments/assets/e8fdf863-d1a9-4005-8883-231c6d856208" />

Imported a 54,000-row IP geolocation watchlist into Sentinel. Used an ipv4_lookup KQL query to enrich raw logs with city, country, latitude, and longitude. This showed exactly where attacks were coming from.

---

## Step 6 – Built the Live Attack Map
<img width="700" src="https://github.com/user-attachments/assets/fba52983-9675-4866-b66d-0b755791e62e" />

Built a custom Sentinel Workbook to visualize attack data on a live world map. The largest clusters came from Czechia (32.6K) and the Netherlands (23.2K), with additional hits from the Philippines, India, South Korea, and the United States.
