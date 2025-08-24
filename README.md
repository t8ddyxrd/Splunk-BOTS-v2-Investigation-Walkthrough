# Splunk-BOTS-v2-Investigation-Walkthrough
A clean, reproducible, GitHub‑ready guide for solving the Splunk BOSS OF THE SOC (BOTS) v2 on your local Splunk. Case Scenario: Command & Control (C2) Beaconing
**Author:** Teodor Todorov  

---

## 🎯 Objective  
Detect potential Command & Control (C2) activity within BOTSv2 by identifying **hosts that beacon to external servers at regular intervals**.
---

## 🔎 Investigation Steps  

### Step 1 – Identify Outbound Connections  

<img width="1919" height="960" alt="Screenshot_20250824_165426" src="https://github.com/user-attachments/assets/ef6d9dae-e416-4d71-b714-4f74dfb45b3a" />

📌 Purpose: Establish baseline of client → server communication.
ps: use | sort by - count in order to find the IP that is being hit the most.


spl

Step 2 – Visualize Connection Frequency
<img width="1920" height="839" alt="Screenshot_20250824_162623" src="https://github.com/user-attachments/assets/e4d6d783-58f4-47f5-b965-d7fc4bb1fe61" />
📌 Purpose: Look for a “heartbeat” pattern that repeats at regular intervals.

spl
Step 3 – Focus on a Suspicious Destination
<img width="1915" height="884" alt="Screenshot_20250824_163602" src="https://github.com/user-attachments/assets/cf346fb1-1453-4673-b598-fe0d42d41388" />
📌 Purpose: Zoom in on the client communicating with a suspicious IP.
```
```
spl
Step 4 – Inspect the Requests
<img width="1920" height="883" alt="Screenshot_20250824_164207" src="https://github.com/user-attachments/assets/fe2d4d35-c83c-4a71-92f3-e209aa40050c" />
📌 Purpose: Check if the requests are repetitive (e.g., same URI or method).

spl
Step 5 – Cross-Check with IDS Alerts (Suricata)
<img width="1920" height="883" alt="Screenshot_20250824_164207" src="https://github.com/user-attachments/assets/4ba3a9cb-1cc2-41d7-82ab-ffdeb446c1fc" />
📌 Purpose: Confirm whether IDS flagged the traffic as malicious.


📝 Findings

Detected a regular beaconing pattern from workstation 172.31.10.10 to suscipious IP 172.31.10.10
-Activity occurred at ~5-minute intervals (indicative of C2 heartbeat).

-HTTP requests were repetitive (same URI/method).

-Suricata confirmed suspicious outbound traffic.

✅ Conclusion

This scenario demonstrates:

Using time-based analysis to detect beaconing.

Pivoting between HTTP logs and IDS alerts.

Correlating internal hosts with suspicious external infrastructure.


