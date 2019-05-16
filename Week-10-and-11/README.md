
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
1) MODERNHONEYNET SERVER SUPERUSER COMMAND LINE PASSWORD RESET:
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
2) LACK OF ATTACK ACTIVITY IN THE MHN SERVER POST HONEYPOT SUCCESSFUL DEPLOYMENT:
   ______________________________________________________________________________
   + After I had deployed the Dionaea, Conpot, Drupot Honeypots I had expected Attack Activity to Reflect in the MHN Server Web Application
     but much to my suprise this did not happen so I started Troubleshooting to Identify the Root Cause of this Issue.
   + Check if the the Honeypots had Registered Successfully with the MHN Server: 
       So If the Honeypot Configuration Files had the Correct MHN Server Information.
       Ex:   /etc/dionaea/dionaea.conf
          hpfeeds = {
			    hp1 = {
				  server = "35.236.80.41" //MHN SERVER EXTERNAL IP ADDRESS
				  port = "10000" //MHN SERVER HPFEEDS TCP PORT 
				  ident = "6f905700-76d3-11e9-95b5-42010aa80003" //HONEYPOT UNIQUE IDENTIFIER 
				  secret = "PRE_SHARED_SECRET" //PRESHARED SECRET BETWEEN THE MHN SERVER AND THE HONEYPOT FOR AUTHENTICATION
			   }
         }
      This Information was Synchronous with the MHN Server Side Information available from
         mhn-admin@mhn-admin-vm:~$ mongo hpfeeds
         MongoDB shell version: 2.6.12
         connecting to: hpfeeds
         > db.auth_key.find()
          { 
            "_id" : ObjectId("5cdba571ef5bf905f331302c"), 
            "subscribe" : [ ], 
            "secret" : "PRE_SHARED_SECRET", 
            "identifier" : "6f905700-76d3-11e9-95b5-42010aa80003", 
            "publish" : [ "mwbinary.dionaea.sensorunique", "dionaea.capture", "dionaea.capture.anon", "dionaea.caputres", "dionaea.connections" ] 
          }

   + To Feed Data to the Modern Honeynet Server the Honeypots and Sensor's relied on the 
     "Honeynet Project Generic Authenticated Datafeed Protocol" which used TCP Port 10000 
     so I had thought this would be a good palce to start from. 
   + Troubleshooting:
     + Modern Honeynet Server Reachability: My first thought was to check if the MHN Server was Responsive to ICMP Ping Request's. 
        ping 35.236.80.41
         PING 35.236.80.41 (35.236.80.41) 56(84) bytes of data.
         64 bytes from 35.236.80.41: icmp_seq=1 ttl=69 time=2.03 ms
         64 bytes from 35.236.80.41: icmp_seq=2 ttl=69 time=0.482 ms
        Since 35.236.80.41, the MHN Server IP was Responsive to ICMP Ping Request's my next guess was to check for the Presence of a Host Based Firewall.
     + Check for the Presence of a Host Based Firewall: Then I thought that A Host Based Firewall like ufw, iptables was Active 
       Restricting Egress Network Traffic thereofre causing this Issue
       but UFW (Uncomplicated Firewall) was Inactive
         +sudo ufw status
           Status: inactive 
      whereas iptables did not have any Active Firewall Rules
        sudo iptables -L
          Chain INPUT (policy ACCEPT)
          target     prot opt source               destination
          sshguard   all  --  anywhere             anywhere

          Chain FORWARD (policy ACCEPT) 
          target     prot opt source               destination

          Chain OUTPUT (policy ACCEPT)
          target     prot opt source               destination

          Chain sshguard (1 references)
          target     prot opt source               destination
      + Network Connectivity for Outbound Traffic on TCP Port 10000: Since 35.236.80.41, the MHN Server IP was Responsive to ICMP Ping's and there was no   Host Based Firewall I thought there were Network Connectivity Issues. 

        I first confirmed if the MHN Server was Listening on TCP Port 10000
          netstat -nat | grep "10000"
          tcp6       0      0 :::10000                 :::*                    LISTEN
      
        So I tried to telnet to the MHN Server's Port 10000 but the Connection Timed out. 
        
          telnet 35.236.80.41 10000
          Trying 35.236.80.41...
          telnet: Unable to connect to remote host: Connection timed out
       
      + Check the Honepot Logs: To dig deeper I had to look into the Honeypot's Execution Log's. 
          Ex1: 
          /var/log/conpot.log 
           2019-05-15 07:20:00,804 connecting to 35.236.80.41:10000
           2019-05-15 07:20:03,810 FeedException while connecting: Could not connect to broker [35.236.80.41].
          It was clear that Issue was in the hpfeed component, although it was Attempting to Connect to the MHN Server's HPFeed Service 
          but it was of no avail.  
          Ex 2: 
          MHN Server HPFeeds Broker Error's:
          tailf /var/log/mhn/hpfeeds-broker.err
          cb(*args)
          File "/opt/hpfeeds/broker/feedbroker.py", line 244, in _newconn
           fc = FeedConn(c, addr, self.db)
          File "/opt/hpfeeds/broker/feedbroker.py", line 84, in __init__
           self.sendinfo()
          File "/opt/hpfeeds/broker/feedbroker.py", line 87, in sendinfo
           self.conn.write(self.msginfo())
          File "/opt/hpfeeds/env/src/evnet-dev/evnet/__init__.py", line 505, in write
           raise EVException('Already closed.')
          EVException: Already closed.

          Honeypot Error's: 
          tailf /var/log/conpot.out
            File "src/gevent/_hub_primitives.py", line 55, in gevent.__hub_primitives.WaitOperationsGreenlet.wait
            File "src/gevent/_waiter.py", line 151, in gevent.__waiter.Waiter.get
            File "src/gevent/_greenlet_primitives.py", line 60, in gevent.__greenlet_primitives.SwitchOutGreenletWithLoop.switch
            File "src/gevent/_greenlet_primitives.py", line 60, in gevent.__greenlet_primitives.SwitchOutGreenletWithLoop.switch
            File "src/gevent/_greenlet_primitives.py", line 64, in gevent.__greenlet_primitives.SwitchOutGreenletWithLoop.switch
            File "src/gevent/__greenlet_primitives.pxd", line 35, in gevent.__greenlet_primitives._greenlet_switch
            timeout: timed out
         WARNING:pyhpfeeds:FeedException while connecting: Could not connect to broker [35.236.80.41].
         2019-05-15 09:05:53,222 FeedException while connecting: Could not connect to broker [35.236.80.41].
         2019-05-15 09:06:13,223 connecting to 35.236.80.41:10000
          Traceback (most recent call last):
           File "/usr/lib/python2.7/SocketServer.py", line 593, in process_request_thread
            self.finish_request(request, client_address)
           File "/usr/lib/python2.7/SocketServer.py", line 334, in finish_request
            self.RequestHandlerClass(request, client_address, self)
           File "/usr/lib/python2.7/SocketServer.py", line 649, in __init__
            self.handle()
           File "/usr/lib/python2.7/BaseHTTPServer.py", line 340, in handle
            self.handle_one_request()
           File "/usr/lib/python2.7/BaseHTTPServer.py", line 310, in handle_one_request
            self.raw_requestline = self.rfile.readline(65537)
           File "/usr/lib/python2.7/socket.py", line 476, in readline
            data = self._sock.recv(self._rbufsize)
           File "/opt/conpot/env/local/lib/python2.7/site-packages/gevent/_socket2.py", line 294, in recv
            return self._sock.recv(*args)
           error: [Errno 104] Connection reset by peer
         The "Connection reset by peer" Message meant that the MHN Server was sending a Packet with the RST Bit Set 
         thereby Closing The Connection, my intuition was that this was the work of a Network Based Firewall.
      + My intuition was correct: I assumed that TCP Ports 3000,10000 were used for Administrating MHN Server 
        but I did not know the exact purpose of these ports. 
        So when I had created the MHN Server VM I had safeguarded it with a Firewall Rule to 
        Only allow Ingress Network Traffic to these TCP Ports from Trusted IP/IP Ranges. 
      + The Honeypot's Relied on the HPFeed Protocol's Outbound Network Communication on TCP Port 10000 
        Since I had Restricted the Ingress Network Traffic to these TCP Ports from Trusted IP's 
        the Honeypot's External IP was unable to communicate with the MHN Server TCP Port 10000.
      + Solution: I modified the Firewall Rule to allow Traffic from the IP Range 0.0.0.0/0
        I verified if the Newly Deployed Firewall Rule had the Intended Result by running
        the following commands on the Honeypot
        + telnet 35.236.80.41 10000
           Trying 35.236.80.41...
           Connected to 35.236.80.41.
           Escape character is '^]'.
        + sudo tcpdump -nnNN tcp port 10000
           tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
           listening on eth0, link-type EN10MB (Ethernet), capture size 262144 bytes
           08:33:40.760025 IP 10.168.0.8.40158 > 35.236.80.41.10000: Flags [S], seq 2731733996, win 28400, options [mss 1420,sackOK,TS val 1516339 ecr 0,nop,wscale 7], length 0
        At the same time I observed the following message on the Honeypot HPFeed Broker Logs
           tailf /var/log/mhn/hpfeeds-broker.err
            "INFO:root:Auth success by mnemosyne."
      after which I started observing the Activity Activity on the MHN Server Dashboard. 






