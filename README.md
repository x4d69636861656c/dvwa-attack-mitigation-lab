# wazuh-detection-hardening-lab
A practical, hands on real-world attack scenarios for learning how to detect post-exploitation activity using **Wazuh**. This repository focuses on real-world attack scenarios, detection engineering, and system hardening.

## Overview
Wazuh is a powerful open-source security monitoring platform used for endpoint detection, file integrity monitoring, log analysis, and threat detection. This lab demonstrates how to use wazuh to detect attacker behavior after initial access has been gained.

This lab emphasizes host-based detection - what happens on the system after an attacker lands.

## Goals
- Learn how to configure Wazuh for real detection use cases
- Understand common post-exploitation techniques
- Practice writing and tuning detections
- Learn practical hardening techniques
- Build a portfolio project that demonstrates real skills

## Lab Environment
- Wazuh Manager: Running in Docker (single-node)
- Target System: Ubuntu VM running DVWA
- Attacker Machine: Kali Linux
- Wazuh Agent: Installed on the Ubuntu VM

## Scenarios Covered
Web Shell Upload -> File Integrity Monitoring (syscheck)
Reverse Shell -> Command monitoring + process

## Repository Structure
wazuh-detection-lab/                                                                                                  
  ├── README.md                                                                                                         
  ├── docs/                                                                                                             
  │   ├── lab-overview.md                                                                                               
  │   ├── architecture.md                                                                                               
  │   └── hardening-guide.md                                                                                            
  ├── configs/                                                                                                          
  │   ├── wazuh-agent/                                                                                                  
  │   └── php/                                                                                                          
  ├── attacks/                                                                                                          
  │   ├── web-shell/                                                                                                    
  │   └── reverse-shell/                                                                                                
  ├── detection/                                                                                                        
  │   └── wazuh-queries.md                                                                                              
  ├── scripts/                                                                                                          
  └── images/        
