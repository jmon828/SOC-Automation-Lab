# Objective
Generate Telemetry from our Windows 10 Machine and ensure that it is being ingested into Wazuh
#  Skills
<ul>
  1. Configured Wazuh<br/>
  2. Sent Telemetry that contained Mimikatz<br/>
  3. Created a custom alert that gets triggered if Mimikatz is activated<br/>
</ul>

# Tools
<ul>
  Mimikatz – is an application that attackers/redteamers use to extract credentials from your machine<br/>
</ul>

# Links
<ul>
  <a href="https://www.youtube.com/watch?v=amTtlN3uvFU&t=3s">1. SOC Automation Project (Home Lab) | Part 4</a>
</ul>

# Steps
<ul>
  <a href="https://github.com/jmon828/SOC-Automation-Lab/blob/main/Part%204:%20Generating%20Telemetry%20and%20Creating%20Custom%20Alerts/Generating%20Telemetry%20and%20Creating%20Custom%20Alerts.md#step-1-configure-windows-10-telemetry">Step 1: Configure Windows 10 Telemetry</a><br/>
  <a href="https://github.com/jmon828/SOC-Automation-Lab/blob/main/Part%204:%20Generating%20Telemetry%20and%20Creating%20Custom%20Alerts/Generating%20Telemetry%20and%20Creating%20Custom%20Alerts.md#step-2-configuring-ossecconfiguration-to-ingest-sysmon-logs">Step 2: Configuring ossec.configuration to ingest Sysmon logs</a><br/>
  <a href="https://github.com/jmon828/SOC-Automation-Lab/blob/main/Part%204:%20Generating%20Telemetry%20and%20Creating%20Custom%20Alerts/Generating%20Telemetry%20and%20Creating%20Custom%20Alerts.md#step-3-installing-mimikatz-on-to-our-vm">Step 3: Installing Mimikatz on to our VM</a><br/>
  <a href="https://github.com/jmon828/SOC-Automation-Lab/blob/main/Part%204:%20Generating%20Telemetry%20and%20Creating%20Custom%20Alerts/Generating%20Telemetry%20and%20Creating%20Custom%20Alerts.md#step-4-creating-a-wazuh-alert">Step 4: Creating a Wazuh alert</a><br/>
  <a href="https://github.com/jmon828/SOC-Automation-Lab/blob/main/Part%204:%20Generating%20Telemetry%20and%20Creating%20Custom%20Alerts/Generating%20Telemetry%20and%20Creating%20Custom%20Alerts.md#step-5-creating-a-new-index">Step 5: Creating a New Index</a><br/>
  <a href="https://github.com/jmon828/SOC-Automation-Lab/blob/main/Part%204:%20Generating%20Telemetry%20and%20Creating%20Custom%20Alerts/Generating%20Telemetry%20and%20Creating%20Custom%20Alerts.md#step-6-troubleshooting-incoming-events">Step 6: Troubleshooting incoming events </a><br/>
  <a href="https://github.com/jmon828/SOC-Automation-Lab/blob/main/Part%204:%20Generating%20Telemetry%20and%20Creating%20Custom%20Alerts/Generating%20Telemetry%20and%20Creating%20Custom%20Alerts.md#step-7-creating-a-rule">Step 7: Creating a Rule</a><br/>
  <a href="https://github.com/jmon828/SOC-Automation-Lab/blob/main/Part%204:%20Generating%20Telemetry%20and%20Creating%20Custom%20Alerts/Generating%20Telemetry%20and%20Creating%20Custom%20Alerts.md#step-8-testing-the-rule">Step 8: Testing the Rule</a><br/>
</ul>

## Step 1: Configure Windows 10 Telemetry<br/>

    Open you VM > File Explorer > File Path: This PC\LocalDisk\Program Files (x86)\ossec-agent\ > ossec .conf > Open with Notepad
![image](https://github.com/user-attachments/assets/79639c1a-e37a-41cc-ad4f-7871eefa6251)
![image](https://github.com/user-attachments/assets/b8fc7652-1c55-4762-a5cf-9515b11e2487)
![image](https://github.com/user-attachments/assets/8cf8569b-7b4b-4a3b-bcc7-735899b6674f)
![image](https://github.com/user-attachments/assets/b29a82fe-43ba-4bd7-a698-f6f3f65ea85d)
![image](https://github.com/user-attachments/assets/fa938a24-5f4a-4cac-8b82-b019b656e588)



This configuration contains everything related to Wazuh.<br/>

Once we open up osse.conf and scroll a little bit, we will see a section called “Log analysis,” and under that section, we see a “<local file>” section that has a “<location>” of security. And under that, we can see some EventIDs being excluded using the exclamation mark.
![image](https://github.com/user-attachments/assets/48a348d2-526e-4f77-acbe-cf64653a0145)

If you wanted to monitor for Powershell (for example) you can copy and follow that same syntax as the <local file>. But in this project, we want to look for processes that involve and contain Mimikatz. In order to do that, we must install Sysmon, which we did on day 2! Since we did that in day 2 lets configure our Ossec.conf file!<br/>


## Step 2: Configuring ossec.configuration to ingest Sysmon logs<br/>
First, it is beast practice to create a back up the config file in case any problems come up
![image](https://github.com/user-attachments/assets/294013ab-55e1-42fe-93d2-54e8bd80dbbd)
![image](https://github.com/user-attachments/assets/6be47028-c11b-44ba-a5f6-a1f0ab738288)


Now lets open up the ossec configuration file
![image](https://github.com/user-attachments/assets/fef4f4aa-a651-4bfe-9db4-ae4e0cacc49e)


Next we want to copy one of the <local file> tags and right underneath it we will paste it
![image](https://github.com/user-attachments/assets/6234f6a4-c6ae-4863-9e47-230fe8dd0509)
![image](https://github.com/user-attachments/assets/f0380c4d-c826-4bab-9ffc-70b15f918357)


Now that we have done that. We can now change the <location> name from “Application” to Sysmon’s channel name. To do that:<br/>

    Event viewer > Expand Applications and Services Logs > Expand Microsoft > Expand Windows >Expand Sysmon > Right Click operational > Properties >  Copy Full name
![image](https://github.com/user-attachments/assets/bb26167e-7183-40a8-bb54-684163ccbcf4)
![image](https://github.com/user-attachments/assets/2b1e0e26-f42f-4d30-be3c-71789df79428)
![image](https://github.com/user-attachments/assets/5a6f9010-8bd6-4b1d-ba76-045b46452506)
![image](https://github.com/user-attachments/assets/c26e212e-b56b-4bcb-b050-b4f5d0bf20ad)



Now that we have copied the full name we can now replace the <location> to the fullname
![image](https://github.com/user-attachments/assets/90b93125-4911-41d5-bd59-6fcc2dbbaf85)
![image](https://github.com/user-attachments/assets/198ae141-0a21-4a8c-a200-60a6ad872d29)


For the sake of ingestion like in the video followed. I will remove local files Application, Security, System and save.
![image](https://github.com/user-attachments/assets/baf1ea27-2c9d-47ed-83f8-4944e11e29ed)
![image](https://github.com/user-attachments/assets/6fcc7fe1-1394-45f4-8eb0-fc58e89f215c)


Removing these essentially means that, they will no longer forward events to our Wazuh server.
If you ever get this error:
![image](https://github.com/user-attachments/assets/c6bb3661-ec0f-4220-8a19-fac08ddbf3ae)

    Copy the Sysmon local file > open administrator notepad > File (Top) > Open > Drop Down – All files > ossec > make changes > save
![image](https://github.com/user-attachments/assets/2187bb71-a69f-47c6-80f4-34c5b3bc48e6)
![image](https://github.com/user-attachments/assets/38d2994d-a9a5-4bbe-ace6-2213f94d6711)



Next we will now restart our Wazuh Service on our VM – Everytime you make a change in the The Wazuh config file you must restart the service

    Open services > Find Wazuh > Right click> Restart
![image](https://github.com/user-attachments/assets/b04e10f7-7c57-436f-a049-df3767fa7cdb)
![image](https://github.com/user-attachments/assets/6f8bb2a1-6b25-4195-a9ec-00e0f0884cd7)


Now Head over to Wazuh and Login.
![image](https://github.com/user-attachments/assets/314c1062-2ca3-4ec9-80aa-257338b328c0)

Switch to the Events tab and ensure we are under “wazuh-alerts” index
![image](https://github.com/user-attachments/assets/70b05367-ae12-4071-b128-e987f1d8cb23)

Next, we can now search for Sysmon events- this might take some time; therefore, if you do not see anything at first, that is perfectly fine!<br/>
![image](https://github.com/user-attachments/assets/6201def2-7271-438f-886d-e7ab02cd8ef3)

## Step 3: Installing Mimikatz on to our VM<br/>
Prior to installing mimikatz, we want to disable Windows Defender or exclude our downloads folder on our VM as it will detect it.
Mimikatz – is an application that attackers/redteamers use to extract credentials from your machine
Open VM > Windows Security > Virus & Threat Protection > Manage threat & Protection Settings: Manage Settings > Exclusions: Add or Remove exclusions > Add an Exclusion: Folder > Select Downloads Folder > Yes
![image](https://github.com/user-attachments/assets/6b7a7d73-3c3a-45cd-9ca4-1a5e71a28a20)
![image](https://github.com/user-attachments/assets/7d3d19bb-d154-4447-acc5-bf48998811d5)
![image](https://github.com/user-attachments/assets/b65007a9-f7c8-4d4f-9791-42a7be7d3a3e)
![image](https://github.com/user-attachments/assets/aaf2bfd0-67d6-40ff-8f4f-45cacb935b44)
![image](https://github.com/user-attachments/assets/efe2d13c-9742-41f1-a1cc-76282123e18a)


Now our Downloads Folder is Excluded!
![image](https://github.com/user-attachments/assets/ccb87189-2388-43a5-b2e7-13bc640fd098)

Now, we download Mimikatz and save it to our downloads folder.
Head over to the Mimikatz GitHub page (in links) > Download mimikatz_trunk.zip > Save it to the downloads folder
![image](https://github.com/user-attachments/assets/86af58e7-b020-4a86-bcdc-8abe810d9c5f)

Once it installed Extract the zip file
![image](https://github.com/user-attachments/assets/fde18fc3-6014-48d8-8f56-abc38d969cdb)
![image](https://github.com/user-attachments/assets/106ad755-2209-4f84-a491-69af1c9d3a5a)
![image](https://github.com/user-attachments/assets/82fc8bba-ba10-4a87-985a-b86c8c98355b)



Once extracted:<br/>

Open an Administrator Powershell > Change directory to Mimikatz file path – command: cd C:\Users\User\Downloads\mimikatz_trunk\x64 > run Mimikatz.exe – command: .\mimikatz.exe
![image](https://github.com/user-attachments/assets/dc070858-5c82-4639-ae1a-3c2eded783a6)
![image](https://github.com/user-attachments/assets/ecfe1dca-060e-44f9-bedc-e747a546c4ed)
![image](https://github.com/user-attachments/assets/82da36d5-a851-4172-a8bb-725c6c9d282c)



## Step 4: Creating a Wazuh alert<br/>

If you log into Wazuh and try searching for Mimikatz. By default, an alert will not show up as Wazuh does not log everything. To fix this we must configure a rule or alert to be triggered.
![image](https://github.com/user-attachments/assets/cd6e21eb-658e-42d3-9c8f-7b9779708d6b)


Modifying the Ossec Configuration to log everything

    Ssh into you Wazuh Manager CLI (I'm using PuTTY) > Copy and paste the ossec.conf file: cp /var/ossec/etc/ossec.conf ~/ossec-backup.conf > Open conf file using nano: nano /var/ossec/etc/ossec.conf
![image](https://github.com/user-attachments/assets/a43db949-7ab6-4a20-8744-d590ba34210c)
![image](https://github.com/user-attachments/assets/749035a1-a82d-4cbb-9e27-359ec3d42bfe)
![image](https://github.com/user-attachments/assets/b420e2a4-3517-48f8-afcf-45fe090e5b4c)



    Once the configuration file has opened> scroll to “<logall>” and “<log_json>” > Change the  “no” to a yes” > Save
![image](https://github.com/user-attachments/assets/d9772132-8edd-40fe-8dba-882943b6bbe3)
![image](https://github.com/user-attachments/assets/519e46e3-b1a7-46b0-ad45-013bdb750710)
![image](https://github.com/user-attachments/assets/b292820f-7d50-42d8-9ed6-5d5f3284d540)


    Once after being saved > Restart the Wazuh Manager: systemctl restart wazuh-manager.service
![image](https://github.com/user-attachments/assets/93cd46f9-6df6-4a94-ab72-5e802eb87ffd)

This forces Wazuh to start archibving all the logs and puts them into a file called “archives”. This file will be located in /var/ossec/logs/archive
![image](https://github.com/user-attachments/assets/9d87fd5f-5bbe-456b-a93a-343fb4803a4b)

For Wazuh to start ingesting these logs we would need to change our configuration in “filebeat”

    SSH > Root> cd /var/ossec/logs/archive > nano /etc/filebeat/filebeat.yml > scroll to “filebeat.modules” > archives enabled: true > Save
![image](https://github.com/user-attachments/assets/f4a7a592-0516-41e7-9ae6-551fd336209b)
![image](https://github.com/user-attachments/assets/dff7039d-0585-477c-ab40-89ce73d93987)


Once we updated the filebeat.yml file we must restart the Wazuh Manager<br/>

    command : systemctl restart filebeat
![image](https://github.com/user-attachments/assets/7af13c8c-e4f2-4a2b-a63a-d1012b9e7580)

## Step 5: Creating a New Index<br/>
    On our Wazuh Dashboard click on the hamburger > Scroll to Stack Management > Click on Index Patterns
![image](https://github.com/user-attachments/assets/9cedd125-6e69-42dd-926e-947a87541913)
![image](https://github.com/user-attachments/assets/f80aa312-ca3d-4e9a-8f5e-e4a3dd15d9dd)
![image](https://github.com/user-attachments/assets/13432ef2-1cb2-4688-b181-543cf14c77c7)




### 5a. we want to create an index for archives, so that we can search all the logs. Regardless if Wazuh triggered an alert.<br/>
    Create index pattern > Index Pattern Name: wazuh-archives-** > Next Step Button > Time field: timestamp > Create index pattern.
![image](https://github.com/user-attachments/assets/6aff11f1-0833-47fa-8971-e19496bfce72)
![image](https://github.com/user-attachments/assets/98cb6e77-8841-4077-acd9-d611324406af)
![image](https://github.com/user-attachments/assets/ca0d39fa-788a-4b5d-9d0a-eef4328cf5bb)



### 5b. head to Discover and filter for archives<br/>

    Top right Hamburger > Discover > Select indexes through the Drop down arrow > wazuh archives
![image](https://github.com/user-attachments/assets/5e0ac41c-e474-43c3-b91f-3939dfbd2e10)
![image](https://github.com/user-attachments/assets/dbbbb1c6-d628-4f5c-bdd2-838e23b89e75)


### Step 6: Troubleshooting incoming events<br/>

By default not all logs will show in the manager. Only those that trigger a rule will show up. That is why we had to configure the logs to log everything. Making it so regardless of a rule being triggered or not we want the manager to archive it and allow us to search for them alert. Wazuh not showing all the logs is great but since we are testing it would hinder us.
SSH into Wazuh Manager> root > cd /var/ossec/logs/archives
![image](https://github.com/user-attachments/assets/8881fb0d-2361-4c5c-8536-8cbc98c038a4)

To Troubleshoot: <br/>

    Root > /var/ossec/logs/archives > cat archives.json | grep -i mimikatz
![image](https://github.com/user-attachments/assets/bc0ae9ed-979b-48b0-890d-cbcdacd7a147)

If you do not see any alerts on Wazuh . Then the mimikatz event did not generate! The next thing we can do is to regenerate it!<br/>

    Windows VM > Admin Powershell > cd C:\Users\User\Downloads\mimikatz_trunk\x64 > .\mimikatz.exe
![image](https://github.com/user-attachments/assets/1cf8310a-2ded-4b6a-a463-4edd84e022b2)

While were on the Windows 10 VM we can also check event viewer to check if Sysmon is capturing mimikatz. We want to look for event ID 1 <br/>
Event viewer > Applications & Services Logs> Microsoft > Windows > Sysmon > Operational > Right-click > Create Custom View > <"All Event IDs">:1 > ok
![image](https://github.com/user-attachments/assets/6e31bbea-aab0-4d51-a2e8-84fe88a118b0)
![image](https://github.com/user-attachments/assets/eb9cc23d-ba46-47af-9b81-7dd2bc3d4a2b)


If you were to double-click on a event that has the technique name Masquerading. It will show that it is recognizing Mimikatz!
![image](https://github.com/user-attachments/assets/aa8865aa-7aec-456c-8f08-fc16d8a63cae)
![image](https://github.com/user-attachments/assets/c7623b5a-c707-426c-8d31-30baa70f125b)


Next we can check our Wazuh Manager once again and grep for mimikatz!<br/>
Wazuh Manager > cat archives.json | grep -i mimikatz
![image](https://github.com/user-attachments/assets/b4a4b181-ccef-4db8-ba39-82896749533b)


This is a good sign as the out put usually means that if we search in the wazuh dashboard for mimikatz it will be there.
![image](https://github.com/user-attachments/assets/da05933f-5d90-406a-9e60-6f7fdff9a576)

We want to zoom in on the alert that consists of eventid 1. As this will show us the process creations
![image](https://github.com/user-attachments/assets/210a678c-be76-473e-94f5-4a679689a70f)

If you expand on this event you can scroll down find the originalFilename
![image](https://github.com/user-attachments/assets/148a5ec6-cb2c-4f03-90a3-458189929115)

We will be using this field to create our alert. If we were to use a field such as “data.win.eventdata.image” an attacker can simply rename mimikatz into mimicow and the alert can be bypassed but if we utilize the original filename regardless of the name change the alert should still be triggered<br/>

### Step 7: Creating a Rule<br/>
Wazuh has built-in tools that we can utilize that we can reference that is stored in /var/ossec/ruleset/rules. This file path is on Wazuh Managers CLI. But since I'm still kind of new to Wazuh I would like to explore Wazuh’s GUI instead!<br/>

    Wazuh Hompage > Click on Drop-down > Management > Rules > Manage Rules Files>
![image](https://github.com/user-attachments/assets/6ffd20b4-8624-4b75-8b6e-1791b9c4f6c7)
![image](https://github.com/user-attachments/assets/354238bd-eda6-45b9-a8c3-89fb14409fd8)
![image](https://github.com/user-attachments/assets/4409a02b-0784-4a83-96f9-4bb030afb900)



Now since we were interested in Sysmon we can look for that by simple searching it!
![image](https://github.com/user-attachments/assets/9a9c8fe6-5f75-4cbb-946a-a392baebdd01)


Immediately we notice that the is a “800-sysmon_id_1.xml”
![image](https://github.com/user-attachments/assets/f267a768-e6dc-47fa-9d45-9b6b2b92d194)

We can take a look inside the file by clicking on the Eye icon
![image](https://github.com/user-attachments/assets/47792ff3-60d9-4fcf-9037-eb61959e5acb)

These are Sysmon rules that are built-in to Wazuh that specifically target eventid: 1 - Ill copy a rule for reference and build it out as a custom rule that detects Mimikatz
![image](https://github.com/user-attachments/assets/da66ce7a-c18a-4a34-9781-36c98e6809dd)

Head back and click on custom rules
![image](https://github.com/user-attachments/assets/eb2acf72-77f7-4a16-a083-4c84dcc69d85)

And immediately we can see 1 “local_rules.xml”
![image](https://github.com/user-attachments/assets/823d7d89-f190-498e-ac21-124dea7a58a1)

We can edit it using the pencil icon
![image](https://github.com/user-attachments/assets/403db748-2099-453d-b2d1-d9f70a1b26a0)

Next we want to paste our reference below the previous rule. Please keep in mind to follow the indentation in rule on top!
![image](https://github.com/user-attachments/assets/be08d769-439e-48a4-b87a-dd5ee79f0982)

To start customizing the rule we should start with the rule id. Custom rule IDs should always start from 100000. If you look at the rule on top, the rule id is equal to 100001. Therefore we will use 100002 instead since 100001 is already taken.<br/>

The Level is the Severity of the alert. The higher the level, the more important the rule needs to be acted upon. The highest level you can add is 15 and for fun ill change the level to 15.
![image](https://github.com/user-attachments/assets/4e4df448-309d-4784-899d-1b2eeae584e3)


For the field name we would want to change it to the original file name. This name is very case sensitive so please ensure that it is the right name or else it will not be triggered<br/>

For the type, the pcre2 will be left as is as it is essentially regex. The “(?i)” is set to ignore the field value and not the field name. previously it was looking for “c | w scripti.exe” but we want to replace it to mimikatz, so that it looks for Mimikatz in the original filename<br/>
![image](https://github.com/user-attachments/assets/e7df3a09-ee32-4451-ae51-d0049f213732)


Below that we want to remove “<options>no_full_log</options>” because we want all the logs <br/>

We also want to change the description to let us know that Mimikatz has been detected
![image](https://github.com/user-attachments/assets/7bdf0155-353d-47c9-957d-1632eb84a23e)


We also want to change the mitre ID to t1003 to indicate that it is a credential dumping technique – Mitre techniques can be found on their website:<a href="https://attack.mitre.org/techniques/>https://attack.mitre.org/techniques/<a>
![image](https://github.com/user-attachments/assets/bf5d5929-065f-454f-9ab5-70079119d6e3)

After that we will save it and restart the Manager
![image](https://github.com/user-attachments/assets/f13d94c8-60d9-4e97-8bb5-9c039107d2ce)
![image](https://github.com/user-attachments/assets/bce67ad2-d894-4bf0-8ea9-7ef376539723)
![image](https://github.com/user-attachments/assets/be966279-654c-41f6-9029-ecd303c08364)



### Step 8: Testing the Rule<br/>
To test the rule We will be changing the filename of mimikatz to see if an alert is triggered. Since we configured the alert to trigger on the original filename and not the imagefile, it should show in our dashboard!<br/>

    Windows 10 VM > Navigate to the Mimikatz.exe > Rename: any name
![image](https://github.com/user-attachments/assets/44a77cd2-fad0-4385-9c8b-fca68da50746)
![image](https://github.com/user-attachments/assets/6b7e5f23-0eac-41fc-b2c9-fa9662d9886c)



Next we want to head to run the renamed Mimkatz and check our dashboard to see if it triggered!<br/>

    Admin Powershell > cd C:\Users\User\Downloads\mimikatz_trunk\x64 > command: .\youareawesome > Check your Dashboard
![image](https://github.com/user-attachments/assets/597ee421-ecf9-4c2d-a539-839bcecaa47a)
![image](https://github.com/user-attachments/assets/0e70fda2-86c8-4afc-81b8-c28e93a89036)

