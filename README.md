Author:  James Donn

This demo requires a few configuration updates in other Apps to work.  This doc breaks down what each dashboard requires.

### Stream Splunk HTTP and User Access Data 

This requires updates to the splunk_app_stream App.

1) To capture data from the loopback address on your Mac, place this file in local/streamfwd.xml:

<?xml version="1.0" encoding="UTF-8"?>
<CmConfig xmlns="http://purl.org/cloudmeter/config" version="6.1.0">
  <Port>8889</Port>
  <UIDirectory>../ui</UIDirectory>
  <DataDirectory>../data</DataDirectory>
  <LogConfig>streamfwdlog.conf</LogConfig>
  <Capture>
    <InterfaceRegex>(en|eth|lo)[0-9]*</InterfaceRegex>
    <Offline>false</Offline>
  </Capture>
</CmConfig>

2) To ensure that the wire input is enabled, place this file in local/inputs.conf 

[streamfwd://streamfwd]
splunk_stream_app_location = http://localhost:8000/en-us/custom/splunk_app_stream/
disabled = 1

Then restart your Splunk instance.

3) Now ensure that you are collecting "response_time" and "src_headers" fields under the http configuration.

Now, all of the dashboards should have data after you log into your Splunk instance.


### System Performance 

This requires updates to the Splunk_TA_nix:

1) For Yosemite users, update line 45 in top.sh:
vi +45 /opt/splunk/etc/apps/Splunk_TA_nix/bin/top.sh
Replace rshrd with vprvt
This may not be a proper replacement, but it fixes it enough to get data in this dashboard.


### Clicks! and Keys!:

--->> Supported on OS X only!!!  Developed using Yosemite.  No plans for further expansion for other OSs. <<---

To enable keyboard tracking, you must allow the Terminal App to control your computer.  To do this:
System Preferences -> Security & Privacy -> Privacy (tab) -> Accessibility (in column) -> Add Terminal.app 

Also be sure to ensure there is a check mark next to it.

These dashboards are fueled by the keystrokes.py script in the bin dir.  The script needs to be launched manually to start tracking clicks.  It takes about 5 seconds to begin recording.  To stop it, you need to kill it. 

Due to its evil nature, I will not start this automatically.  This is also why the default data retention time for keystrokes is very low.  Feel free to change any of this on your own.

        /opt/splunk/etc/apps/stream_splunk_usage_demo/bin/keystrokes.py &


### Wish list:
Tag Cloud dash for most popular words
Add trackpad distance traveled
Add window resizing
Force Directed dash  based on mouse movement
Add simple / verbose check box for RT keystrokes
