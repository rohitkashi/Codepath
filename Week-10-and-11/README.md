
+ COURSE:Codepath Cybersecurity
+ WEEK(S): 10,11
+ PROJECT: Honeypot
+ AUTHOR: Rohit Chandra Tejaswi Kashibatla 
+ STUDENT ID: 012530830
+ EMAIL:  rohitchandratejaswi.kashibatla@sjsu.edu
+ CLOUD HOSTING PROVIDER: Google Cloud Platform

DEPLOYED HONEYPOT'S AND SENSOR'S: 
_________________________________
1) "Ubuntu - Dionaea with HTTP" (Honeypot) 
   "Ubuntu - Snort" (Intrustion Detection and Prevention Sensor) 
   "Ubuntu - p0f" (TCP/IP Fingerprint Sensor)
2) "Ubuntu - Conpot" (Industrial Control System Honeypot)
   "Ubuntu - Snort" (Intrustion Detection and Prevention Sensor) 
   "Ubuntu - p0f" (TCP/IP Fingerprint Sensor)
3) "Ubuntu - Drupot" (Drupal Content Management System Web Form Bot Spam Honeypot )
   "Ubuntu - Snort" (Intrustion Detection and Prevention Sensor)
   "Ubuntu - p0f" (TCP/IP Fingerprint Sensor)

SUMMARY OF DATA CAPTURED FROM HONEYPOTS AND SENSORS:
____________________________________________________
+ ATTACKS in the last 24 hours: 8,122
+ TOP 5 Attacker IPs:
   + 38.141.45.50 (2,784 attacks)
   + 77.247.110.36 (989 attacks)
   + 111.230.73.43 (615 attacks)
   + 75.25.131.207 (344 attacks)
   + 193.188.22.115 (220 attacks)
+ TOP 5 Attacked ports:
   + 5060 (3,878 times)
   + 80 (745 times)
   + 8088 (543 times)
   + 3389 (297 times)
   + 23 (245 times)
+ TOP 5 Honey Pots:
   + dionaea (6,841 attacks)
   + drupot (626 attacks)
   + snort (343 attacks)
   + p0f (277 attacks)
   + conpot (35 attacks)
+ TOP 5 Sensors:
   + mhn-ubuntu-1404-dionaea-honeypot-with-http (6,841 attacks)
   + mhn-ubuntu-1404-drupal-cms-webform-bot-spam-honeypot (626 attacks)
   + mhn-ubuntu-1404-dionaea-honeypot-with-http (281 attacks)
   + mhn-ubuntu-1404-conpot-ics-honeypot (160 attacks)
   + mhn-admin-vm (114 attacks)
+ TOP 5 Attacks Signatures:
   + ET SCAN SipCLI VOIP Scan (147 times)
   + ET DROP Dshield Block Listed Source group 1 (85 times)
   + ET CINS Active Threat Intelligence Poor Reputation IP TCP group 81 (26 times)
   + ET CINS Active Threat Intelligence Poor Reputation IP TCP group 80 (17 times)
   + ET CINS Active Threat Intelligence Poor Reputation IP TCP group 37 (9 times)
+ Total HoneyPot Activity Period: 24 Hours
+ Data Captured from Honeypot's and Sensor's: session.json

ISSUES
______
1) MODERNHONEYNET SERVER SUPERUSER PASSWORD RESET:
   I had Forgotten the Superuser Password which was set during the Installation Phase of the Modern Honeynet Admin Application Setup: 
   + In the Setup of the Modern Honeynet Admin Application the User is prompted to set a Password for the Superuser.
   + I had the Setup a Password but had forgotten it. 
   + MHN Troublehshooting Guide to the Rescue: https://github.com/threatstream/mhn/wiki/MHN-Troubleshooting-Guide 
   + The "I can't reset my password because I fat fingered the email during the install" Section in the MHN Troubleshooting
     Guide suggests using the Python Script manual_password_reset.py to Reset the Superuser Password
     "python /opt/mhn/server/manual_password_reset.py"
     but the Default Ubuntu 14.04 LTS VM Image 
     available from Google Cloud Platform Compute Engine 
     used to create the Modern Honeynet Server did not have the following Python Packages which were Required by the Password Reset Script 
     + flask_security
     + flask_sqlalchemy
     + xmltodict
     + celery
     + bson
     + mongoengine
     + pygal
     + bcrypt
     Preventing it from Executing Correctly.
   + After installing these Python Packages using the PIP Package Manager [Command: "pip install PACKAGE_NAME --user"]
     I was able to Reset the Superuser Password. 
   + The Super User's Password can be Reset from The Modern Honeynet Web Application
     by going to URL: http://SERVER_IP/ui/add-user/ -> Change Password
     but this relied on the following Mail Server Field's to be set in the Setup Stage of the Modern Honeynet Admin Application  
     + Mail server address ["localhost"]:
     + Mail server port [25]:
     + Use TLS for email?: y/n n
     + Use SSL for email?: y/n n
     + Mail server username [""]:
     + Mail server password [""]:
     + Mail default sender [""]:
     I did not set the correct fields, hence I could do Reset the Password from the Web Application. 


