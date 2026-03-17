# 🕵️‍♂️ Threat Hunt Report: Bridge Takeover

## 🎯 Scenario

Five days after the file server breach, threat actors returned with sophisticated tools and techniques. 
The attacker pivoted from the compromised workstation to the CEO's administrative PC, deploying persistent backdoors and 
exfiltrating sensitive business data, including financial records and password databases.

## 💻 Compromised System

azuki-adminpc

## 🔎 Evidence Available

Microsoft Defender for Endpoint Logs

## Flags:

### 🔹 Flag 1 - LATERAL MOVEMENT - Source System
**Objective**: Identify the source IP address for lateral movement to the admin PC.

**KQL Query**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-16%20215746.png?raw=true)

**Results:**

![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-16%20220034.png?raw=true)

**Finding**:
- Source IP: 10.1.0.204

### 🔹 Flag 2 - LATERAL MOVEMENT - Compromised Credentials
**Objective**: Identify the compromised account used for lateral movement

**KQL Query**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20085034.png?raw=true)

**Results:**
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20085107.png?raw=true)

**Finding**:
- **Account**: yuki.tanaka

### 🔹 Flag 3 – LATERAL MOVEMENT - Target Device
**Objective**: What is the target device name

**KQL Query**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20090347.png?raw=true)

**Results:**
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20090416.png?raw=true)

**Finding**:
- **Device Name**: azuki-adminpc

### 🔹 Flag 4 – EXECUTION - Payload Hosting Service
**Objective**: What file hosting service was used to stage malware

**KQL Query**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20100039.png?raw=true)

**Results:**
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20100103.png?raw=true)

**Finding**:
- **Hosting URL**: litter.catbox.moe



