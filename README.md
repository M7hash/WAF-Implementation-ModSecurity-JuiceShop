# WAF Implementation using ModSecurity and OWASP Juice Shop

## 📌 Project Overview
This project demonstrates the implementation of a Web Application Firewall (WAF)
using ModSecurity and OWASP Core Rule Set (CRS) to protect a vulnerable application
(OWASP Juice Shop).

## 🧰 Tools & Technologies
- Kali Linux
- Apache2
- ModSecurity
- OWASP CRS
- Docker
- OWASP Juice Shop

## 🧪 Attacks Tested
- Cross Site Scripting (XSS)
- SQL Injection (SQLi)
- Command Injection
- Local File Inclusion (LFI)

## 🔐 Results
All malicious requests were detected and blocked with HTTP 403 responses.
Detailed audit logs were generated and analyzed.

## 📸 Screenshots
Screenshots of attack execution and WAF blocking are available.

## Project Implementation Steps

## Step 1: Environment Setup
Updated the system and installed required packages.

'''bash 
sudo apt update
sudo apt install apache2 libapache2-mod-security2 docker.io -y 
'''

## Verification:
sudo systemctl status apache2

## Step 2: Enable ModSecurity
Enabled ModSecurity module in Apache.

sudo a2enmod security2
sudo systemctl restart apache2

## Verification:
apachectl -M | grep security

## Step 3: Download OWASP CRS
sudo apt install modsecurity-crs -y

## Step 4: Configure OWASP Core Rule Set (CRS)
Configured ModSecurity to use OWASP CRS.

sudo cp /etc/modsecurity/modsecurity.conf-recommended /etc/modsecurity/modsecurity.conf
sudo sed -i 's/SecRuleEngine DetectionOnly/SecRuleEngine On/' /etc/modsecurity/modsecurity.conf
sudo systemctl restart apache2

## Verification:
Apache starts without errors
CRS rules load successfully

## Step 5: Deploy OWASP Juice Shop
Deployed OWASP Juice Shop using Docker.

sudo docker run -d -p 3000:3000 --name juice-shop bkimminich/juice-shop

## Verification:
Access Juice Shop at:
http://localhost:3000

## Step 6: Configure Apache as Reverse Proxy
Configured Apache to forward traffic to Juice Shop so WAF inspects requests.

sudo a2enmod proxy proxy_http
sudo systemctl restart apache2

## Verification:
Juice Shop accessible via Apache
ModSecurity intercepts requests

## Step 7: Perform Attack – XSS Example
Tested Cross-Site Scripting attack.

Payload Used:
<script>alert(1)</script>


## Result:
Request blocked
HTTP 403 Forbidden returned

## Step 8: Log Analysis
Analyzed ModSecurity audit logs.
sudo tail -f /var/log/apache2/modsec_audit.log

