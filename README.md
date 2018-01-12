# guest-wifi-scheduler
Web-based tool that allows users to enable / disable a guest wifi network

# Use case:
This tool will run on a minimal server and enables any user with login priviledges to enable/disable a guest wireless network running on a Cisco Wireless LAN controller.  Once scheduled, the user and IT team will be emailed the SSID and password.  The password will be rolled every time.

# Assumptions:
There is an AD server (can easily be modified for LDAP as well) to authenticate to.
The user's login ID is the same as their password.
The network device is a Cisco Wireless LAN controller.  Although this can easily be modified to work with others as well, as long as they are supported by netmiko
The server hosting the application is Linux.  This was implemented no CentOS7.

# Requirements:
*Apache  
*Python3.6  
*SSL certificate  

# Python Requirements:
##pip3 install of the following:  
*sqlite3  
*datetime  
*pandas  
*netmiko  
*flask  
*gunicorn  

# IMPLEMENTATION:
/bin/pip3.6 install sqlite3 datetime pandas netmiko flask  
cp wifi-scheduler /opt/  
cp wifi-enabler.service /etc/systemd/system/  
cp wifi-webfront.service /etc/systemd/system/  
chmod 644 /opt/wifi-scheduler/app.py /opt/wifi-scheduler/wsgi.py /opt/wifi-scheduler/wifi_enabler.py /opt/wifi-scheduler/emailsend.py  
vi /opt/wifi-scheduler/deets.py  #ENTER your email, login info, WLC IP address  
yum -y install httpd  
cp wifi-scheduler.conf /var/httpd/conf.d/  
vi /var/httpd/conf.d/wifi-scheduler.conf #ENTER the config for your implentation -- hostname, AD info, etc...  
systemctl daemon-reload  
systemctl enable wifi-enabler.service  
systemctl enable wifi-webfront.service  
systemctl start wifi-enabler.service  
systemctl start wifi-webfront.service  
  
# Fruits of thine labor:
Enjoy never being bothered to enable the Guest Wifi again.  Take the afternoon off, you've earned it my friend.
