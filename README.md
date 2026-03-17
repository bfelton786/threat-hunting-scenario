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
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-16%20172626.png?raw=true)
