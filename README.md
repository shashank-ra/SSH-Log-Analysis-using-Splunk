# SSH-Log-Analysis-using-Splunk
Splunk SIEM project for SSH log analysis — threat detection, brute-force detection, and security monitoring.

Objective
The goal of this project is to analyze SSH authentication logs to detect and investigate key security events such as:
1. Successful logins – who connected and from where
2. Failed login attempts – possible brute-force or password spraying
3. Multiple failed authentication attempts – indicators of brute-force attacks
4. Connections without authentication – potential port scanning or incomplete sessions
-->This project demonstrates SOC Analyst–level practical skills using Splunk for log ingestion, analysis, dashboards, and real-time alerting.

Lab Setup & Prerequisites

1. Splunk Installation: Completed (Enterprise/Free version)
2. Log File: ssh_log.json
3. Splunk App: Search & Reporting

Steps to Upload Logs

1. Log in to your Splunk instance
2. Go to Apps → Search & Reporting
3. Click Add Data → Upload → Browse → select ssh_log.json
4. Set sourcetype = _json
5. Choose Index name = ssh_logs
6. Click Start Searching

Implementation Tasks

Task 1: Ingest & Parse Logs
-->Ensure the following fields are extracted correctly:

event_type
auth_success
auth_attempts
id.orig_h → Source IP
id.resp_h → Destination Host

Validation Search:
index=ssh_logs | stats count by event_type

<img width="1896" height="1001" alt="1" src="https://github.com/user-attachments/assets/062dbd98-d5a7-4d3d-b8e5-1a7494ac8ee9" />
<img width="1872" height="528" alt="2" src="https://github.com/user-attachments/assets/a245b9b9-8dd2-443e-8148-6ac2bff2fc54" />


Task 2: Analyze Failed Login Attempts
-->Identify failed SSH login attempts.

index=ssh_logs event_type="Failed SSH Login"
| stats count by id.orig_h

Actions:

1. Highlight the top 10 source IPs with failed logins
2. Create a bar chart visualization to display results
-->Helps detect brute-force or password spraying attempts.

<img width="1883" height="711" alt="2" src="https://github.com/user-attachments/assets/7f3b1ad6-6ee6-4338-b30e-f1a185f24c14" />
<img width="1881" height="860" alt="1" src="https://github.com/user-attachments/assets/48169c56-6660-4a5b-8cfd-f534942245e7" />


Task 3: Detect Multiple Failed Authentication Attempts

index=ssh_logs event_type="Multiple Failed Authentication Attempts"
| stats count by id.orig_h, id.resp_h

Configure an Alert:
-->Trigger when any IP attempts more than 5 logins within 10 minutes
-->This helps detect potential brute-force attacks in real time.

<img width="1876" height="895" alt="3" src="https://github.com/user-attachments/assets/f64f7ce6-c5e5-476f-8e03-4436a520bb29" />
<img width="1260" height="841" alt="2" src="https://github.com/user-attachments/assets/8a43f5ca-db8c-40c3-8d53-155a50f4f0e2" />


Task 4: Track Successful Logins

Search:
index=ssh_logs event_type="Successful SSH Login"
| stats count by id.orig_h, id.resp_h

Actions:
1. Compare successful logins with previous failed attempts
2. Create a dashboard panel to track successful logins by source IP
-->Useful for identifying compromised accounts after brute-force success.
<img width="1915" height="885" alt="3" src="https://github.com/user-attachments/assets/292daf5e-0a1a-4a8a-9ffe-34e0239ae1ff" />
<img width="830" height="873" alt="2" src="https://github.com/user-attachments/assets/9ddb2b32-d046-4653-ba45-72e5cf4c2384" />
<img width="1885" height="887" alt="1" src="https://github.com/user-attachments/assets/a89b580b-257e-4396-b572-1a3b978e4084" />



Task 5: Spot Suspicious Connections Without Authentication

Search:
-->index=ssh_logs event_type="Connection Without Authentication"
| stats count by id.orig_h

Timechart Visualization:
index=ssh_logs event_type="Connection Without Authentication"
| timechart count by id.orig_h

-->Detect repeated unauthenticated sessions — often a sign of SSH probing or port scanning.
<img width="1895" height="386" alt="2" src="https://github.com/user-attachments/assets/0d6d6db2-ac31-49f9-bd9a-e95f06fdc15b" />
<img width="1887" height="862" alt="1" src="https://github.com/user-attachments/assets/93c76a2d-d0f4-4f28-ad00-d1b2f8b1ceb4" />


Conclusion
-->By completing this project, you have:
1. Built Splunk dashboards to monitor SSH activity
2. Detected brute-force and suspicious login attempts
3. Configured real-time Splunk alerts for risky behavior
4. Strengthened your Blue Team / SOC Analyst investigation skills

Tools & Technologies
1. Splunk SIEM – Log ingestion, search, and alerting
2. SSH Logs (JSON format) – Authentication event data
3. Data Visualization & Dashboards – For monitoring patterns
4. Real-Time Alerts – For threat detection and response

References & Learning Resources
-->This project was inspired and guided by tutorials from Rajesh Gupta – YouTube Channel
which helped in understanding Splunk search queries, dashboard creation, and alert configuration for SSH log analysis.


Author
Shashank R Achanur
Aspiring SOC Analyst | Blue Team Enthusiast
shashankra680@gmail.com
