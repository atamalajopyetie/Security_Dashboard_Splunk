# Security_Dashboard_Splunk
Built a unified security dashboard in Splunk for network, Linux, and brute-force logs. Configured forwarders, created indexes, and set up daily reports and a real-time dashboard for monitoring events by protocol, severity, and type.
###What did I do ?
- Forwarded logs from endpoints:
  - Suricata (eve.json, fast.log) → Network events
  - /var/log/auth.log → Brute-force login attempts
  - /var/log/syslog → Linux system logs

- Created indexes and configured Splunk forwarder to send logs to the Splunk indexer.

- Simplified log searches using keywords (Failed password, Accepted password, error, warning) for immediate results.

- Built reports and scheduled them daily to track security events automatically.

- Designed a dashboard with:
 - Recent events table (all logs combined)

 - Network logs by protocol

The result: A real-time, unified security overview that’s easy to monitor and extend.

###How to Configure This From Scratch
1. Install Splunk Universal Forwarder on your machine:

    wget -O splunkforwarder.tgz 'https://www.splunk.com/...'
    tar xvzf splunkforwarder.tgz
    ./splunkforwarder/bin/splunk start --accept-license
    
2. Configure inputs.conf to monitor logs:

    [monitor:///var/log/suricata/eve.json]
    disabled = false
    index = network_logs
    sourcetype = suricata:json
    
    [monitor:///var/log/auth.log]
    disabled = false
    index = bruteforce_logs
    sourcetype = linux:auth
    
    [monitor:///var/log/syslog]
    disabled = false
    index = linux_logs
    sourcetype = syslog
    
3. Create indexes on the Splunk indexer:
network_logs, bruteforce_logs, linux_logs

4. Forward events to the indexer (127.0.0.1:9997) using outputs.conf.

5. Run searches in Splunk:

    index=bruteforce_logs "Failed password" | stats count
    index=network_logs | stats count by proto
    index=linux_logs ("error" OR "warning") | stats count by severity
    
6. Save reports and schedule them daily via Splunk’s “Save As → Report → Schedule” option.

7. Create dashboard panels:
 - Table for recent events
 - Charts for aggregated counts by protocol, event type, and severity
 - Add a time picker for dynamic filtering
   
<img width="1280" height="720" alt="Screenshot from 2025-08-27 20-52-20" src="https://github.com/user-attachments/assets/4849784a-e71f-4013-a2ec-55ef2dbe1bf9" />

<img width="1280" height="720" alt="Screenshot from 2025-08-27 20-52-53" src="https://github.com/user-attachments/assets/8998416b-4c02-4c6d-9ec1-ab58563ca403" />

<img width="1858" height="776" alt="Screenshot from 2025-08-27 21-13-21" src="https://github.com/user-attachments/assets/31782cfe-304b-4b3b-80d9-1656657009da" />

<img width="1851" height="778" alt="Screenshot from 2025-08-27 21-36-46" src="https://github.com/user-attachments/assets/4224a6c4-1ad5-43d5-9ae4-d8c79e46303c" />







