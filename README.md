# DVWA Attack & Mitigation Lab
A hands-on lab for learning web application attacks using DVWA, with a focus on understanding security controls, bypassing them, and mitigating attacks using real-world tools like ModSecurity.

## Goals
- Master web application attacks accross different difficulty levels
- Understand how security controls work and how to bypass them
- Learn real-world monitoring and mitigation techniques (ModSecurity, WAFs, etc.)
- Build practical offensive and defensive skills

## Repository Structure
dvwa-attack-mitigation-lab/                                                                                           
├── README.md                                                                                             
└── docs/                                                                                          
    └── command-injection/                     
        ├── command-injection-low.md                                                                                  
        ├── command-injection-medium.md                                                                               
        ├── command-injection-high.md                                                                                 
        └── command-injection-impossible.md

Each attack has its own folder, with separate files for each difficulty level:

- Attack: how to exploit the vulnerability
- Security controls: What DVWA adds at each difficulty level and how to bypass it
- Monitor: How to detect the attack using logs or ModSecurity
- Mitigate: How to block or reduce the rist using DVWA controls and real-world tools.

## Current Attacks
- Command Injection (Low, Medium, High, Impossible)

## Tools Used
- DVWA (Damn Vulnerable Web Application)
- ModSecurity (Web Application Firewall)
- Apache + PHP