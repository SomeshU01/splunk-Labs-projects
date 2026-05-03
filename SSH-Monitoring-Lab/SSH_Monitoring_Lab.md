# SSH Monitoring Dashboard — Splunk Lab

*Step-by-Step Configuration Guide*

---

## Objective

Build a Splunk dashboard to monitor SSH authentication activity, detect brute-force attempts, visualize login trends, and geo-locate attack sources.

---

## Lab Set Up

Ensure Splunk is running and SSH log sources are correctly ingested before proceeding.

---

## Task 0: Setting up Time Range

### 1. Add Time Range Button

- Click on **Add Input**
- Select **Time** and click on the **pencil icon**
- Set Label to `Time Range` and Token to `time_range`
- Again click **Add Input**
- Select **Submit**

> **Note:** For all future panels, set the time to `time_range` for consistency.

---

## Task 1: Authentication Overview Panels

**Goal:** Give a quick summary of SSH activity.

### 1. Total SSH Events

- Click on **Add Panel**
- Under New, choose **Single Value**
- Use Shared Time Picker: `time_range`
- Set Content Title to: `Total SSH Events`
- Enter the Search String:

```spl
source="ssh_logs.json" host="LinuxServer" sourcetype="_json"
| stats count AS "Total SSH Events"
```

### 2. Successful Logins

- Click on **Add Panel**
- Under New, choose **Single Value**
- Use Shared Time Picker: `time_range`
- Set Content Title to: `Successful Logins`
- Enter the Search String:

```spl
source="ssh_logs.json" host="LinuxServer" sourcetype="_json" event_type="Successful SSH Login"
| stats count AS "Successful Logins"
```

### 3. Failed Logins

- Click on **Add Panel**
- Under New, choose **Single Value**
- Use Shared Time Picker: `time_range`
- Set Content Title to: `Failed Logins`
- Enter the Search String:

```spl
source="ssh_logs_new.json" host="LinuxServer" sourcetype="_json" event_type="Failed SSH Login"
| stats count AS "Failed Login"
```

### 4. Connection without Authentication

- Click on **Add Panel**
- Under New, choose **Single Value**
- Use Shared Time Picker: `time_range`
- Set Content Title to: `Invalid User Attempts`
- Enter the Search String:

```spl
index=auth "sshd" "invalid user"
| stats count AS "Invalid User Attempts"
```

### Task 1 — Dashboard Output

![Task 1 Authentication Overview](images/task1_overview.jpg)

---

## Task 2: Login Activity Trends

**Goal:** Visualize login behavior over time and detect spikes.

### 1. Failed Logins by Username

- Click on **Add Panel**
- Under New, choose **Bar Chart**
- Use Shared Time Picker: `time_range`
- Set Content Title to: `Failed Logins by username`
- Enter the Search String:

```spl
source="ssh_logs_new.json" host="LinuxServer" sourcetype="_json" event_type="Failed SSH Login"
| top username
```

### 2. Possible Brute Force

- Click on **Add Panel**
- Under New, choose **Statistics Table**
- Use Shared Time Picker: `time_range`
- Set Content Title to: `Possible Brute Force by IP Address`
- Enter the Search String:

```spl
source="ssh_logs_new.json" host="LinuxServer" sourcetype="_json" event_type="Multiple Failed Authentication Attempts"
| top id.orig_h
```

### Task 2 — Dashboard Output

![Task 2 Login Activity Trends](images/task2_login_trends.jpg)

---

## Task 3: Visualizing Brute Force Attack in Geo-Location

### 1. Brute Force Attack with Geo-Location

- Click on **Add Panel**
- Under New, choose **Choropleth Map**
- Use Shared Time Picker: `time_range`
- Set Content Title to: `Brute Force attack with geo-location`
- Enter the Search String:

```spl
source="ssh_logs_new.json" host="LinuxServer" sourcetype="_json" event_type="Multiple Failed Authentication Attempts"
| table id.orig_h
| iplocation id.orig_h
| stats count by Country
| geom geo_countries featureIdField="Country"
```

### Task 3 — Geo-Location Map Output

![Task 3 Geo Location Map](images/task3_geo_map.jpg)

### Full Dashboard View

![Full Dashboard](images/task3_full_dashboard.jpg)

---

*— End of Lab —*
