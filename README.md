# Guest Wifi Scheduler

This tool runs on a Linux server and allows anyone with an AD account to schedule the Guest WIFI for up to 8 hours.  The WIFI password is rolled each time and emailed to the user & the IT team.

<p align="center">
  <img src="https://github.com/lileddie/guest-wifi-scheduler/blob/master/images/guest-wifi-scheduler.png" alt="screenshot">
</p>

### Prerequisites

To run properly we need Apache and Python3.  This installation used a CentOS7 host.

Apache requires the tools.conf file and the following modules:
```
LoadModule ldap_module modules/mod_ldap.so
LoadModule authnz_ldap_module modules/mod_authnz_ldap.so
LoadModule ssl_module modules/mod_ssl.so
```

The credentials need to be supplied to bind to AD.  These should be added to /etc/httpd/conf.d/deets - restrict access to deets via:
```
chmod chmod o-rwx deets
```

Update the credentials for the gmail account as well - file deets.py - also update emailAddrs if anyone else wishes to be added to notifications.

Python3's requirements can be satisfied with:
```
yum -y install python36u python36u-pip
/usr/bin/pip3 install requirements.txt
```

Make sure SSL cert & key are copied to directory
The cert and key would then need to be copied to /etc/ssl/certs/

### Installation

Copy the files to the appropriate place
```
mkdir /opt/wifi-scheduler
mv guest-wifi-scheduler /opt/wifi-scheduler
cd /opt/wifi-scheduler/
mv tools.conf /etc/httpd/conf.d/
mv wifi-enabler.service /etc/systemd/system/
mv wifi-webfront.service /etc/systemd/system/
```
Site-specific modifications:
```
app.py:
37 - Needs the name of your SSID
94 - Needs the IP address of your cisco WLC
97 - Enter WLAN number from your cisco cisco WLC

deets.py:
Update with your WLC and gmail info.

tools.conf:
26-28: Update with your SSL cert info
37-40: Update with your AD info

wifienabler.py:
update instances of YOUR_IP with the IP of the WLC
Verify the WLAN # is the same as you are using
```


Reload Apache and Start Services
```
systemctl restart httpd
systemctl daemon-reload
systemctl enable wifi-enabler
systemctl enable wifi-webfront
systemctl start wifi-enabler
systemcty start wifi-webfront
```

Update the DNS Alias to point to the new server.

## Verify tool is up and running.

Login to https://wifi-enabler.example.local with your AD password.
You should now be able to run a test Schedule to verify functionality.

### End to end tests

A successful test would include:
 * User can login with AD password.
 * WIFI is scheduled & shows up under [View Schedule](https://wifi-enabler.example.local/view_schedule)
 * User receives an email from gmail with correct password.
 * WIFI network is removed upon schedule completion.

## Built With

* [CentOS7](https://www.centos.org/) - the brains
* [Apache2](https://httpd.apache.org/) - the beauty
* [Python3](https://www.python.org/) - the brawn

## License

This project is licensed under the Apache license - see the [https://github.com/lileddie/guest-wifi-scheduler/LICENSE.md](LICENSE.md) file for details
