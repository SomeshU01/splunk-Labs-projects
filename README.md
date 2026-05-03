# 🛡️ Splunk Security Monitoring Projects

A collection of hands-on Splunk labs focused on security monitoring, threat detection, and log analysis using real-world datasets.

---

## 📁 Repository Structure

```
splunk-security-projects/
│
├── README.md
│
├── SSH-Monitoring-Lab/
│   ├── SSH_Monitoring_Lab.md
│   └── images/
│       ├── task1_overview.jpg
│       ├── task2_login_trends.jpg
│       ├── task3_geo_map.jpg
│       └── task3_full_dashboard.jpg
│
└── (more projects coming soon...)
```

---

## 📌 Projects

| # | Project | Description | Status |
|---|---------|-------------|--------|
| 1 | [SSH Brute Force Monitoring](#1-ssh-brute-force-monitoring) | Detect SSH brute-force attacks, failed logins, and geo-locate attackers using Splunk | ✅ Complete |
| 2 | More Projects | New Splunk labs will be added here as they are completed | 🔜 Coming Soon |

---

## 1. SSH Brute Force Monitoring

> **Lab:** SSH Monitoring Dashboard — Splunk Lab

### 📋 Overview

This lab builds a full Splunk dashboard to monitor SSH authentication activity on a Linux server. It detects brute-force attempts, visualizes login trends, and maps attacker locations using geo-location data.

### 🎯 Goals

- Monitor total SSH events in real time
- Track successful and failed login attempts
- Identify brute-force attacks by IP address and username
- Visualize attack origins on a world map

### 🗂️ Tasks Covered

| Task | Description |
|------|-------------|
| Task 0 | Setting up Time Range input |
| Task 1 | Authentication Overview Panels (Single Value tiles) |
| Task 2 | Login Activity Trends (Bar Chart + Statistics Table) |
| Task 3 | Brute Force Geo-Location Map (Choropleth Map) |

### 🔍 Key SPL Queries Used

**Total SSH Events**
```spl
source="ssh_logs.json" host="LinuxServer" sourcetype="_json"
| stats count AS "Total SSH Events"
```

**Failed Logins**
```spl
source="ssh_logs.json" host="LinuxServer" sourcetype="_json" event_type="Failed SSH Login"
| stats count AS "Failed Login"
```

**Brute Force by IP**
```spl
source="ssh_logs_new.json" host="LinuxNew" sourcetype="_json" event_type="Multiple Failed Authentication Attempts"
| top id.orig_h
```

**Geo-Location Map**
```spl
source="ssh_logs_new.json" host="LinuxNew" sourcetype="_json" event_type="Multiple Failed Authentication Attempts"
| table id.orig_h
| iplocation id.orig_h
| stats count by Country
| geom geo_countries featureIdField="Country"
```

### 📸 Dashboard Screenshots

**Task 1 — Authentication Overview**
![Task 1](SSH-Monitoring-Lab/images/task1_overview.jpg)

**Task 2 — Login Trends & Brute Force Table**
![Task 2](SSH-Monitoring-Lab/images/task2_login_trends.jpg)

**Task 3 — Geo-Location Map**
![Task 3](SSH-Monitoring-Lab/images/task3_geo_map.jpg)

**Full Dashboard View**
![Full Dashboard](SSH-Monitoring-Lab/images/task3_full_dashboard.jpg)

### 📄 Full Lab Guide

👉 [View Full Step-by-Step Guide](./SSH-Monitoring-Lab/SSH_Monitoring_Lab.md)

---

## 🔜 Coming Soon

> The following project slots are reserved for future Splunk labs.
> Each will be added with full documentation, SPL queries, and screenshots.

| # | Topic | Description |
|---|-------|-------------|
| 2 | Windows Event Log Monitoring | Detect suspicious logins and privilege escalation on Windows |
| 3 | Network Traffic Analysis | Monitor unusual network activity and port scanning |
| 4 | Malware Detection | Identify indicators of compromise from endpoint logs |
| 5 | Web Application Attacks | Detect SQL injection, XSS, and other web attacks |
| 6 | Threat Intelligence Integration | Enrich alerts with threat feeds and IOC matching |
| 7 | Custom Alerts & Notifiers | Build Splunk alerts with email and webhook notifications |

> ⚠️ Topics above are placeholders — actual projects may differ based on lab availability.

---

## 🛠️ Tools & Technologies

![Splunk](https://img.shields.io/badge/Splunk-000000?style=for-the-badge&logo=splunk&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![JSON](https://img.shields.io/badge/JSON-000000?style=for-the-badge&logo=json&logoColor=white)

- **Splunk Enterprise** — Log ingestion, search, and dashboards
- **SPL** (Search Processing Language) — Queries and data analysis
- **Linux SSH Logs** — Authentication log source
- **Splunk iplocation** — Geo-location enrichment
- **Choropleth Maps** — Geographic attack visualization

---

## 👤 Author

> Feel free to connect and follow for more security projects!

---

## ⭐ How to Use This Repo

1. Browse the project folders above
2. Open any project's `.md` file for the full step-by-step guide
3. Copy the SPL queries directly into your Splunk search bar
4. Replicate the dashboard panels using the instructions provided

---

*More labs will be added regularly. Star ⭐ this repo to stay updated!*
