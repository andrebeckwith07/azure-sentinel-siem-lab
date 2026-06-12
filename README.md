# Azure Sentinel SIEM Home Lab – Honeypot & Attack Map

## Overview
Deployed a Windows 10 honeypot VM in Azure, forwarded security logs to Microsoft Sentinel, and built a live attack map to visualize real-world brute-force attacks by geolocation.

**Tools used:** Microsoft Azure, Microsoft Sentinel, Log Analytics Workspace, KQL, Windows 10

---

## Architecture
![Architecture Diagram](Screenshot 2026-06-12 165529.png)

Real-world attackers on the public internet hit the honeypot VM through an intentionally open NSG (Network Security Group). The VM forwards its security logs to a Log Analytics Workspace, which feeds into Microsoft Sentinel (the SIEM). Sentinel then powers the live attack map showing where attacks are coming from globally.

---

## Step 1 – Deployed the Honeypot VM
![Honeypot VM](1-honeypot-vm.png)
Deployed a Windows 10 VM in Azure configured as a honeypot. Set the Network Security Group to allow all inbound traffic to expose it to the internet and attract real attackers.

## Step 2 – Observed Failed Login Attempts
![Event Viewer](2-event-viewer.png)
Opened Event Viewer on the honeypot VM and found Event ID 4625 — failed login attempts from real attackers trying to brute-force their way in within hours of the machine going live.

## Step 3 – Queried Logs with KQL
![KQL Query](3-kql-query.png)
Forwarded security logs from the VM to a Log Analytics Workspace. Used KQL (Kusto Query Language) to query failed login attempts — the same query type used by real SOC analysts.

## Step 4 – Connected Microsoft Sentinel
![Sentinel Connected](4-sentinel-connected.png)
Connected Microsoft Sentinel as the SIEM and linked it to the Log Analytics Workspace. Configured the Windows Security Events via AMA data connector to pull logs into the SIEM.

## Step 5 – Enriched Logs with Geolocation
![Enriched Logs](5-enriched-logs.png)
Imported a 54,000-row IP geolocation watchlist into Sentinel. Used an ipv4_lookup KQL query to enrich raw logs with location data — showing which countries the attacks came from.

## Step 6 – Built the Live Attack Map
![Attack Map](6-attack-map.png)
Built a custom Sentinel Workbook to visualize attack data on a live world map. Each point represents a source of brute-force login attempts targeting the honeypot in real time.
