# Cibrary

## Ethical Hacking

Certified Ethical Hacker (CEH)
Compuer Hacking Forensic Investigator (CHFI)

### Module 1: The Basics

a. OSI
b. TCP/IP

#### CIA Black White Grey Hats

1. Ethical Hacker <--> Penetration tester

__Black Hat__: Criminal Hacker
__Grey Hacker__: You don't have permissions but you do good stuff like: Fix vulnerable devices.
__White Hack__: You have permissions

2. IAM:

Identity and Access Management: Providing the right people with the right access

3. C.I:A Triad

a. Confidentiality
b. Integrity
c. Availability

4. Authentication - Non repudiation

5. Physical security

a. Procedures
b. Physical measures
c. Operational measures: policies and procedures

6. AI and Cyber Security

AI can potentially notice threats, detect vulnerabilities and so on
Is it gonna work?. We don't know: it is not the greatest yet

#### Laws

1. HIPAA

Health Insurance Portability and Accountability Act
Safeguarding private medical information

2. PCI-DSS

Payment card Industry - Data Security Standard

a. Debit card transactions
b. Protect Cardholder Data

3. SOX

Sarbanes-Oxley Act
Related with Enron
Security for finantial reports

4. DMCA

Dygital Millenium Copyright Act
Owner of the content. No violation of content

5. FISMA

Federal Information Security Management Act
Review Security practices

6. ISO/IEC 27001: 2013

Management needs to:

- Examine information security risk
- Design security controls
- Implement Security controls

#### VB and Kali

Those are required tools

#### Password Crack Lab Instructions

John the ripper
Using kali linux

```
john --format=raw-md5 --wordlist /usr/share/wordlists/rockyou.txt /home/kali/Desktop/password.txt
```


#### Footprinting Basics

1. Classification

a. Active:
- interaction with our target
- port scan

b. Pasive:

- public information

2. Benefits

- Know security posture
- Reduce focus area
- Identify vulnerabilities
- Network map

3. How?

- Seach engines
- Google hacking
- Whois
- Shodan
- Job boards -> it will give you different information about what a company is running
- visualping.io
- Website mirror: httrack
- Email footprinting -> the Harvester

4. Tools

Maltego: mapping certain stuff
Recon-ng
OSR - Framework

5. Using nikto

`nikto -e 1 -h www.google.com`

6. Harvester

Gathering information about domains, subdomains, banners

theHarvester -d ebay.com -l 150 -b google

7. Shodan

Hackers search engine
