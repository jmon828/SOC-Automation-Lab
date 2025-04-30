## Objective
Generate Telemetry from our Windows 10 Machine and ensure that it is being ingested into Wazuh
## Skills
Configured Wazuh
Sent Telemetry that contained Mimikatz
Created a custom alert that gets triggered if Mimikatz is activated
## Tools
Mimikatz – is an application that attackers/redteamers use to extract credentials from your machine

## Links


## Steps
Step 1: configure Windows 10 Telemetry
Open you VM > File Explorer > File Path: This PC\LocalDisk\Program Files (x86)\ossec-agent\ > ossec .conf > Open with Notepad




This configuration contains everything related to Wazuh.
Once we open up osse.conf and scroll a little bit, we will see a section called “Log analysis,” and under that section, we see a “<local file>” section that has a “<location>” of security. And under that, we can see some EventIDs being excluded using the exclamation mark.

If you wanted to monitor for Powershell (for example) you can copy and follow that same syntax as the <local file>. But in this project, we want to look for processes that involve and contain Mimikatz. In order to do that, we must install Sysmon, which we did on day 2! Since we did that in day 2 lets configure our Ossec.conf file!
Step 2: Configuring ossec.configuration to ingest Sysmon logs
First, it is beast practice to create a back up the config file in case any problems come up


Now lets open up the ossec configuration file

Next we want to copy one of the <local file> tags and right underneath it we will paste it


Now that we have done that. We can now change the <location> name from “Application” to Sysmon’s channel name. To do that:
Event viewer > Expand Applications and Services Logs > Expand Microsoft > Expand Windows >Expand Sysmon > Right Click operational > Properties >  Copy Full name



Now that we have copied the full name we can now replace the <location> to the fullname


For the sake of ingestion like in the video followed. I will remove local files Application, Security, System and save.


Removing these essentially means that, they will no longer forward events to our Wazuh server.
If you ever get this error:

Copy the Sysmon local file > open administrator notepad > File (Top) > Open > Drop Down – All files > ossec > make changes > save



Next we will now restart our Wazuh Service on our VM – Everytime you make a change in the The Wazuh config file you must restart the service
Open services > Find Wazuh > Right click> Restart


Now Head over to Wazuh and Login.

Switch to the Events tab and ensure we are under “wazuh-alerts” index

Next, we can now search for Sysmon events- this might take some time; therefore, if you do not see anything at first, that is perfectly fine!

Step 3: Installing Mimikatz on to our VM
Prior to installing mimikatz, we want to disable Windows Defender or exclude our downloads folder on our VM as it will detect it.
Mimikatz – is an application that attackers/redteamers use to extract credentials from your machine
Open VM > Windows Security > Virus & Threat Protection > Manage threat & Protection Settings: Manage Settings > Exclusions: Add or Remove exclusions > Add an Exclusion: Folder > Select Downloads Folder > Yes





Now our Downloads Folder is Excluded!

Now, we download Mimikatz and save it to our downloads folder.
Head over to the Mimikatz GitHub page (in links) > Download mimikatz_trunk.zip > Save it to the downloads folder

Once it installed Extract the zip file



Once extracted:
Open an Administrator Powershell > Change directory to Mimikatz file path – command: cd C:\Users\User\Downloads\mimikatz_trunk\x64 > run Mimikatz.exe – command: .\mimikatz.exe



Step 4: Now we can move on to Creating a Wazuh alert
If you log into Wazuh and try searching for Mimikatz. By default, an alert will not show up as Wazuh does not log everything. To fix this we must configure a rule or alert to be triggered.

Modifying the Ossec Configuration to log everything
Ssh into you Wazuh Manager CLI (I'm using PuTTY) > Copy and paste the ossec.conf file: cp /var/ossec/etc/ossec.conf ~/ossec-backup.conf > Open conf file using nano: nano /var/ossec/etc/ossec.conf



Once the configuration file has opened> scroll to “<logall>” and “<log_json>” > Change the  “no” to a yes” > Save


Once after being saved > Restart the Wazuh Manager: systemctl restart wazuh-manager.service

This forces Wazuh to start archibving all the logs and puts them into a file called “archives”. This file will be located in /var/ossec/logs/archive

For Wazuh to start ingesting these logs we would need to change our configuration in “filebeat”
SSH > Root> cd /var/ossec/logs/archive > nano /etc/filebeat/filebeat.yml > scroll to “filebeat.modules” > archives enabled: true > Save


Once we updated the filebeat.yml file we must restart the Wazuh Manager- command : systemctl restart filebeat

Step 5: Create a new index
On our Wazuh Dashboard click on the hamburger > Scroll to Stack Management > Click on Index Patterns




b. we want to create an index for archives, so that we can search all the logs. Regardless if Wazuh triggered an alert.
Create index pattern > Index Pattern Name: wazuh-archives-** > Next Step Button > Time field: timestamp > Create index pattern.



C. head to Discover and filter for archives
Top right Hamburger > Discover > Select indexes through the Drop down arrow > wazuh archives


Step 6: Troubleshooting incoming events
By default not all logs will show in the manager. Only those that trigger a rule will show up. That is why we had to configure the logs to log everything. Making it so regardless of a rule being triggered or not we want the manager to archive it and allow us to search for them alert. Wazuh not showing all the logs is great but since we are testing it would hinder us.
SSH into Wazuh Manager> root > cd /var/ossec/logs/archives

To Troubleshoot:
Root > /var/ossec/logs/archives > cat archives.json | grep -i mimikatz

If you do not see any alerts on Wazuh . Then the mimikatz event did not generate! The next thing we can do is to regenerate it!
Windows VM > Admin Powershell > cd C:\Users\User\Downloads\mimikatz_trunk\x64 > .\mimikatz.exe

While were on the Windows 10 VM we can also check event viewer to check if Sysmon is capturing mimikatz. We want to look for event ID 1
Event viewer > Applications & Services Logs> Microsoft > Windows > Sysmon > Operational > Right-click > Create Custom View > <All Event IDs> : 1 > ok


If you were to double-click on a event that has the technique name Masquerading. It will show that it is recognizing Mimikatz!


Next we can check our Wazuh Manager once again and grep for mimikatz!
Wazuh Manager > cat archives.json | grep -i mimikatz

This is a good sign as the out put usually means that if we search in the wazuh dashboard for mimikatz it will be there.

We want to zoom in on the alert that consists of eventid 1. As this will show us the process creations

If you expand on this event you can scroll down find the originalFilename

We will be using this field to create our alert. If we were to use a field such as “data.win.eventdata.image” an attacker can simply rename mimikatz into mimicow and the alert can be bypassed but if we utilize the original filename regardless of the name change the alert should still be triggered
Step 7: Creating a Rule
Wazuh has built-in tools that we can utilize that we can reference that is stored in /var/ossec/ruleset/rules. This file path is on Wazuh Managers CLI. But since I'm still kind of new to Wazuh I would like to explore Wazuh’s GUI instead!
Wazuh Hompage > Click on Drop-down > Management > Rules > Manage Rules Files>



Now since we ware instrested in Sysmon we can look for that by simple searching it!

Immediately we notice that the is a “800-sysmon_id_1.xml”

We can take a look inside the file by clicking on the Eye icon

These are Sysmon rules that are built-in to Wazuh that specifically target eventid: 1 - Ill copy a rule for reference and build it out as a custom rule that detects Mimikatz

Head back and click on custom rules

And immediately we can see 1 “local_rules.xml”

We can edit it using the pencil icon

Next we want to paste our reference below the previous rule. Please keep in mind to follow the indentation in rule on top!

To start customizing the rule we should start with the rule id. Custom rule IDs should always start from 100000. If you look at the rule on top, the rule id is equal to 100001. Therefore we will use 100002 instead since 100001 is already taken.
The Level is the Severity of the alert. The higher the level, the more important the rule needs to be acted upon. The highest level you can add is 15 and for fun ill change the level to 15.

For the field name we would want to change it to the original file name. This name is very case sensitive so please ensure that it is the right name or else it will not be triggered
For the type, the pcre2 will be left as is as it is essentially regex. The “(?i)” is set to ignore the field value and not the field name. previously it was looking for “c | w scripti.exe” but we want to replace it to mimikatz, so that it looks for Mimikatz in the original filename

Below that we want to remove “<options>no_full_log</options>” because we want all the logs
We also want to change the description to let us know that Mimikatz has been detected

We also want to change the mitre ID to t1003 to indicate that it is a credential dumping technique – Mitre techniques can be found on their website:

After that we will save it and restart the Manager



Step 7: Testing the Rule
To test the rule We will be changing the filename of mimikatz to see if an alert is triggered. Since we configured the alert to trigger on the original filename and not the imagefile, it should show in our dashboard!
Windows 10 VM > Navigate to the Mimikatz.exe > Rename: any name


Next we want to head to run the renamed Mimkatz and check our dashboard to see if it triggered!
Admin Powershell > cd C:\Users\User\Downloads\mimikatz_trunk\x64 > command: .\youareawesome > Check your Dashboard

