# Objective:
Connect Shuffle (SOAR Platform) to send alerts to TheHive and send an alert to SOC Analyst (ME) via Email.

# Skills:
<ul>
    1. Configured Automation with Shuffle<br/>
    2. Configured Case Management tool, TheHive<br/>
    3. Integrated Virus Total in Automation tool<br/>
    4. Utilized ChatGPT for Code assistance<br/>
</ul>

    
# Tools:
<ul>
    1. Shuffle- Security Orchestration, Automation, and Response Tool<br/>
    2. TheHive- Collaborative Case Management Platform <br/>
    3. VirusTotal- Free File Malware scanner <br/>
    4. ChatGPT - AI Assistant/chatbot <br/>
    5. Temp-mail.io – Temporary mailbox <br/>
    6, Pyinstaller – Converts script into an executable<br/>
</ul>

# Links:
<ul>
    <a href="https://www.youtube.com/watch?v=GNXK00QapjQ&t=61s">1. SOC Automation Project (Home Lab) | Part 5</a><br/>
    <a href="https://medium.com/@kumarr71621/enhanced-security-incident-management-with-wazuh-and-soar-automation-fe9b69bbf127">2. Enhanced Security Incident Management with WAZUH and SOAR Automation by. Rajat Kumar</a>
</ul>

# Steps:
<ul>
     <a href="https://github.com/jmon828/SOC-Automation-Lab/blob/main/Part%205:%20Integrating%20Wazuh,%20TheHive%20&%20Shuffle/Part%205:%20Integrating%20Wazuh,%20TheHive%20&%20Shuffle.md#step-1-setting-up-shuffle">Step 1: Setting up Shuffle</a><br/>
     <a href="https://github.com/jmon828/SOC-Automation-Lab/blob/main/Part%205:%20Integrating%20Wazuh,%20TheHive%20&%20Shuffle/Part%205:%20Integrating%20Wazuh,%20TheHive%20&%20Shuffle.md#step-2-connecting-the-wazuh-manager-to-shuffle">Step 2: Connecting the Wazuh Manager to Shuffle</a><br/>
     <a href="https://github.com/jmon828/SOC-Automation-Lab/blob/main/Part%205:%20Integrating%20Wazuh,%20TheHive%20&%20Shuffle/Part%205:%20Integrating%20Wazuh,%20TheHive%20&%20Shuffle.md#step-3-creating-a-workflow">Step 3: Creating a Workflow</a><br/>
     <a href="https://github.com/jmon828/SOC-Automation-Lab/blob/main/Part%205:%20Integrating%20Wazuh,%20TheHive%20&%20Shuffle/Part%205:%20Integrating%20Wazuh,%20TheHive%20&%20Shuffle.md#step-4-setting-up--api-to-check-reputation-score">Step 4: Setting up API to check reputation score</a><br/>
     <a href="https://github.com/jmon828/SOC-Automation-Lab/blob/main/Part%205:%20Integrating%20Wazuh,%20TheHive%20&%20Shuffle/Part%205:%20Integrating%20Wazuh,%20TheHive%20&%20Shuffle.md#step-5-implementing-thehive">Step 5: Implementing theHIVE</a><br/>
     <a href="https://github.com/jmon828/SOC-Automation-Lab/blob/main/Part%205:%20Integrating%20Wazuh,%20TheHive%20&%20Shuffle/Part%205:%20Integrating%20Wazuh,%20TheHive%20&%20Shuffle.md#step-6-create-an-email-the-contains-relevant-information-to-send-to-a-security-analyst">Step 6: Creating an email to send to a Security Analyst</a><br/>
     <a href="https://github.com/jmon828/SOC-Automation-Lab/blob/main/Part%205:%20Integrating%20Wazuh,%20TheHive%20&%20Shuffle/Part%205:%20Integrating%20Wazuh,%20TheHive%20&%20Shuffle.md#step-7-active-response-setup">Step 7: Active Response Setup</a><br/>
</ul>


## Step 1: Setting up Shuffle
    Head to  site > Create an account > Click on Work flows on the Left-hand side
![image](https://github.com/user-attachments/assets/d79ce88c-2d18-46fe-a9bf-140c2de743e1)

To create a workflow Click on “Create Workflow button”
![image](https://github.com/user-attachments/assets/ee2f8fbb-287f-433d-b20e-7fa90ed268b6)

You will be prompted to set a Name & Use Case, like below!
![image](https://github.com/user-attachments/assets/144871e8-522f-4bc0-8082-f4cc00750a26)

You will now be shown a “Change Me” workplace where you can start adding apps and triggers
![image](https://github.com/user-attachments/assets/bfbd71fc-a29b-4ce9-adc7-b1ff5806ca71)

On the Left hand-Side there is a section called "Triggers” This section includes a couple of Webflow Starters
![image](https://github.com/user-attachments/assets/25f8c21e-be29-4da4-ab22-c133a824c7aa)

Drag and Drop Webhook ad Click on it
![image](https://github.com/user-attachments/assets/398fe1a2-664c-419b-bb6e-b75b72210e23)

Once Selected Rename it to Wazuh-Alerts and Copy the Webhook URI – We need to copy it so that we can paste it onto our ossec file hosted on the Wazuh Manager
![image](https://github.com/user-attachments/assets/47211056-4971-4bc8-a651-ef056a3c2198)

Prior to Configuring the Wazuh Manager we should verify that the change me icon is set to “Repeat back to me” and ensure that the Call Section is Runtime Argument ($exec) and Save it!
![image](https://github.com/user-attachments/assets/766ca879-ea0d-4ade-af3e-b52a6c6de55b)


## Step 2: Connecting the Wazuh Manager to Shuffle
    SSH into Wazuh Manager > nano into ossec file “nano /var/ossec/etc/ossec.conf”
![image](https://github.com/user-attachments/assets/21dcb7c8-0138-43cb-bbc2-4a18c5d6247e)

Scroll down after the </global> tag. Copy and paste the integrating tag (can be found ) underneath the <global> tag. – Replace the default <hook_url>  with the webhook URI (the one we copied earlier)
![image](https://github.com/user-attachments/assets/90d49f7d-0906-4548-aaaf-5bcca0364244)

By default, it has a level of three, which means that any alert with a level of three will be sent to Shuffle. We don't want this; instead, we want to change the level to trigger by the rule ID that we configured, 1002, which is our mimikatz alert. - We can do this by replacing the level tag with rule_id
![image](https://github.com/user-attachments/assets/574d499d-de7d-4c85-9375-cac7da36b707)

Once were done we will save it and restart the wazuh-manager.service
![image](https://github.com/user-attachments/assets/1e62b333-b4a9-437e-af40-7923bf77a99d)

We can also verify that the service is running by the command systemctl status wazuh-manager.service
![image](https://github.com/user-attachments/assets/51bafa38-615b-435c-b818-41c55a86f8c8)

Once we have confirmed that the service is active, Next we will regenerate the Mimikatz Telemetry on our Windows 10 VM

    Login to Windows 10 VM > Admin Powershell > cd C:\Users\User\Downloads\mimikatz_trunk\x64 > .\youareawesome.exe
![image](https://github.com/user-attachments/assets/411f3c56-20f5-4705-a71e-9d82155c2144)

Next we want to head to our shuffle instance and Test our workflow

    Login to Shuffle > Open your Workflow > Click on Webhook Icon > click Start on the right-hand side
![image](https://github.com/user-attachments/assets/2b33e738-82f5-496c-917f-df219f4056f9)

Next Click on the running icon at the bottom and test the workflow
![image](https://github.com/user-attachments/assets/b5ae0f16-624d-4f54-b08f-22058fa0626a)

Ensure you have $exec as runtime argument<br/>
![image](https://github.com/user-attachments/assets/cf02b564-db52-411a-86dd-4f8017d10027)


Inspect the run<br/>
![image](https://github.com/user-attachments/assets/ee9ba4f8-ce8d-42f2-a81c-a0d47c7317de)

Expand the results and you get the results that was generated from Wazuh!
![image](https://github.com/user-attachments/assets/ad7d334b-810c-4201-93d0-ddca5cf6260d)

## Step 3: Creating a Workflow

    Note: the workflow we will be creating will be designed around this demo. But the same concepts can be applied to other use cases!
<ul>
Workflow Summary:<br/>
1. Mimikaztz Alert sent to Shuffle<br/>
2. Shuffle will receive the Mimikatz Alert – Extract SHA256 Hhash from File<br/>
3. Check the Reputation score w/VirusTotal<br/>
4. Send Details to TheHive to Create Alert<br/>
5. Send Email to SOC Analyst to Begin Investigation<br/>
</ul>

When we look at the return values for the hashes we can see that it is appended by their hash type<br/>
![image](https://github.com/user-attachments/assets/aa373af2-3884-48e6-b21d-15312f3fcfdc)

If we wanted to automate this we would need to parse out the hash value itself because if we don’t do that step the entire value including the “SHA1=” will be sent to VirusTotal to get a reputation score. We don’t want that, we just want the Hash value itself. To do this we must:

    Go into Shuffle > Click on Change me icon > Rename it to SHA-256 > Change Repeat back to me to “Regex capture group” >
![image](https://github.com/user-attachments/assets/10526603-f87e-43b6-84ac-320bebd08e53)

    Input Data: Hashes > utilize chat GPT to create a Regex to parse the SHA256 hash > Copy and Paste the Created regex into the Regex Box > Save
![image](https://github.com/user-attachments/assets/40a445df-daa2-4c8b-a756-5ff29d67311d)
![image](https://github.com/user-attachments/assets/cb8ee343-3fb2-4509-b1fd-b48f7f5c206b)
![image](https://github.com/user-attachments/assets/49778780-a042-418f-ac6f-ea20db125de8)




Once Saved Click on the Workflow run icon > Rerun the workflow <br/>
![image](https://github.com/user-attachments/assets/a2c3cb89-858f-44c1-af21-05b708aee8fe)
![image](https://github.com/user-attachments/assets/734c5732-1d37-44f6-b952-4b33c8c0169f)


Once Rerun is done, we can scroll to the SHA-256 result, expand it and notice that it parsed out the SHA256 hash!
![image](https://github.com/user-attachments/assets/3b16560d-648a-42e5-9010-6f1247a10a61)

## Step 4: Setting up  API to check reputation score
    Prerequisite: Create a VirusTotal account

Copy VirusTotal Key
![image](https://github.com/user-attachments/assets/e2306e1d-8a42-454a-b992-db9819b7b40e)
![image](https://github.com/user-attachments/assets/7b893cda-3766-47e8-b67d-42bbe3d64d73)


### 4b. Head back to shuffle and add the Virus Total App
![image](https://github.com/user-attachments/assets/d7aa5ec9-67cb-4696-aaf0-6ba59d3f3bb9)

    Once the app is Activated > Drag and drop the App on to our workflow
![image](https://github.com/user-attachments/assets/a705dcb4-d358-4871-8d52-fd2e950c9bc6)

### 4c. Configure VirusTotal App
![image](https://github.com/user-attachments/assets/d6ac1639-c250-4f01-81e2-78961a35f690)

Click on Authenticate Virustotal V3 to apply the API key you copied earlier and submit
![image](https://github.com/user-attachments/assets/7c5930a6-bd2a-4c09-b3a7-8060b5e7619d)
![image](https://github.com/user-attachments/assets/704b87a3-a92c-4548-ac94-8f3eacd84bb8)


Once Submitted > Ensure that the ”ID” is set to the Regex output. Remember the regex is there to parse out the value of the hash!
![image](https://github.com/user-attachments/assets/e24f2ddb-3d2a-4767-8e1e-bcbab73de4aa)

### 4d. VirusTotal Testing and Troubleshooting
Once you have saved the workflow click on the run icon to show previous workflows<br/>
![image](https://github.com/user-attachments/assets/3a289e57-ac2a-446b-ad00-f4d3987ba8c7)

Click on a recent workflow and rerun
![image](https://github.com/user-attachments/assets/b563295f-aff8-44ec-b5e2-ec753ff19420)
![image](https://github.com/user-attachments/assets/7afb296c-28e7-4c14-b068-1e3ebfefba93)


Once the Rerun is finished, expand the Virustotal window
![image](https://github.com/user-attachments/assets/506d1608-60f2-4e57-ac00-0ff3f6f9b077)


If you have no hash at the end of the “url” section – follow the video at 15:50
Once you have done that and re-ran the work flow you should see the results below
![image](https://github.com/user-attachments/assets/e27ef85a-6df2-487c-8a08-458715821429)

Analyzing Further if we expand the Attributes Section
![image](https://github.com/user-attachments/assets/310edff5-3766-44f3-8297-a584655b3ff3)

Then scroll to the “last_analysis_stats” Section
![image](https://github.com/user-attachments/assets/97d438ad-ca6d-4223-9193-4ab962325cac)

We can see that there is a number 64 items for the malicious field
![image](https://github.com/user-attachments/assets/c3695417-07f1-4eb3-bb66-104dae75c7f9)

This means that there were 64 scanners that marked this as malicious

## Step 5: Implementing theHIVE
### 5a. Add TheHive application to our workflow
![image](https://github.com/user-attachments/assets/e15fd9ab-3832-4097-a9f6-85c15e0b6d68)


### 5b. configuring TheHive Server
Head to TheHive website & login
![image](https://github.com/user-attachments/assets/fd6f64eb-a4c3-4c91-a7ef-056e459cce27)

If you are getting any sort of authentication error, this usually means that elasticsearch isn’t running. You might have to remote into it and start the service again. – this was done in Day 3
![image](https://github.com/user-attachments/assets/028b7342-a676-430a-aac8-ce0bb345971a)


### 5c. Adding another organization and creating a new user under that organization
By default we only have one organization. We can add another by clicking the plus icon on the top left
![image](https://github.com/user-attachments/assets/ee303a1d-80e3-485e-9374-cf106af525af)

Next we fill in the Name and Description fields and confirm
![image](https://github.com/user-attachments/assets/8fb05664-7c8e-4512-97ee-8d35b90a0800)

Once that is done click on it and immediately we are greeted with a “no users have been found” screen
![image](https://github.com/user-attachments/assets/56ada1b3-b4a0-4905-9a76-d8628a37b798)
![image](https://github.com/user-attachments/assets/9475b905-8e44-4ed8-8e55-ec0ba54b2d0f)


To add a user click on the “+” icon
![image](https://github.com/user-attachments/assets/727d6353-bd43-439c-85df-79ad116bd588)

When prompted we can now create a user
![image](https://github.com/user-attachments/assets/e0763d5a-31be-4107-8266-8f28f9dd7507)

Next I will create another account but this time it will be used as a service account
![image](https://github.com/user-attachments/assets/7376b5cc-4496-4c70-859d-cb7638720ac1)

At the end the Organization should have two users:
![image](https://github.com/user-attachments/assets/6bbdf8be-bc70-4256-871e-41fb9b681e6f)

### 5d. Applying passwords to our users
Hover over the mydfir account and select preview
![image](https://github.com/user-attachments/assets/e62d6950-b52a-4c25-a4f3-d4ef8037c270)

Scroll down and click on “set password” – Add a password and confirm
![image](https://github.com/user-attachments/assets/949a61c1-d4c8-4461-90ec-cbefcad9cb47)
![image](https://github.com/user-attachments/assets/d980fbf9-064e-4de1-ba54-96c8d8f2b18b)


### 5e. Creating an API Key for the SOAR user (admin)
Hover over the SOAR user and click on preview
![image](https://github.com/user-attachments/assets/aa14d449-af37-4e5e-8143-c19985f9f4b1)

Scroll a bit to “API Key” section and click on create
![image](https://github.com/user-attachments/assets/56d94171-3ffa-4f34-8b2d-ccc586cea6a0)

Copy and paste the key onto a notepad as we will be using it later on!

### 5f. Verifying user accounts work
Login using the username and password that we created
![image](https://github.com/user-attachments/assets/41b50c19-2d56-4858-a009-2255e5810f62)

No cases have been found should be the default
![image](https://github.com/user-attachments/assets/d2dddf65-56cb-4781-a611-3a75b3fd3e40)

5g. Setting up TheHive in our Workflow
Log back into shuffle and double click on TheHive app that we placed on our workflow
![image](https://github.com/user-attachments/assets/9554763e-8b25-4aad-aec3-251d59e50a25)

Click on Authenticate TheHive
![image](https://github.com/user-attachments/assets/43cc57dc-b912-4973-a5dd-77d08501b25b)

Paste the API Key that we copied earlier & for the URL, paste the public IP of your TheHive Instance along with the port number – Then hit Submit
![image](https://github.com/user-attachments/assets/bf2a2f7f-042b-468b-a5d3-5480eff80f6d)

Now under the find actions we want to “create an alert” – click on the drop down, and select Create Alert
![image](https://github.com/user-attachments/assets/3e156b23-45ce-4be3-aa29-4570ef73eab9)

Now we scroll a bit to fill in the fields.
When following MYDFIR’s video I came across an issue where I didn’t have the same field as him. To fix this, I added another line to the JSON code to add the field I was missing. <br/>

I did this by:
Head to TheHive App > Clicking on Create Alert action > Followed the JSON format and add the missing field – in this case, I was missing the “date” field therefore I added the “date”: “${date}” JSON line (See Below) > Then Submit.
![image](https://github.com/user-attachments/assets/cba2569b-f313-4da7-b9d5-d3669f7e3d3e)
![image](https://github.com/user-attachments/assets/abc7bb7e-8699-492c-a454-98095c774bc4)


Head back into our Workflow and we can now see the new “Date” field
![image](https://github.com/user-attachments/assets/2d145bd4-f894-4b00-9243-690a931d712d)

Now, Selecting the date we can now add a Runtime argument
Click on the “+” icon beside the Field
![image](https://github.com/user-attachments/assets/968bfe19-6277-4eff-bd49-bbc256ea45d1)

Hover over Runtime Argument and Select utcTime
![image](https://github.com/user-attachments/assets/b9475bbb-7d3c-43bd-b065-5b7caa43ffb6)
![image](https://github.com/user-attachments/assets/96639034-5d89-49ba-b9c2-629b0387d07b)

Next do the Following:
For description, Type the following:

    Mimikatz Detected on host:$exec.text.win.system.computer from user: $exec.text.win.eventdata.user
![image](https://github.com/user-attachments/assets/f8a990d2-33b6-49e2-9ba1-4121b68521de)
![image](https://github.com/user-attachments/assets/98da1c96-b05d-4a98-a3c0-99c9da52cd52)
![image](https://github.com/user-attachments/assets/f2726a9b-1987-4987-816e-6eac405f347d)

<ul>
    pap = permissible actions protocol (level of exposure of information)
    Source = where the alert is coming from
    Sourceref= metadata for rule
    Status= status of the alert that will be created in TheHive
    Summary = Similar to Description
    TLP = Traffic light protocol = confidentiality of information
    Type = where did the alert come from
</ul>

### 5h. Configuring Cloud firewall to allow all IP’s coming inbound on port 9000 (where our Hive Instance will live.
    Login to Digital Ocean > Networking > Firewalls > Select the firewall that we created
![image](https://github.com/user-attachments/assets/8e306c79-11bf-4b4c-896d-010f14e11f2d)

    Click on new rule > and click custom
![image](https://github.com/user-attachments/assets/c1c56b03-b469-4e96-96a2-4073e4763cc2)

    > Change the port to 9000 > Remove All IPv6 > save
![image](https://github.com/user-attachments/assets/e360c8e0-1323-46f3-98de-ee3b9cfef7a8)



### 5i. Testing and Verifying
    Go back into TheHive > Click on the “Running Man icon” > Rerun workflow
![image](https://github.com/user-attachments/assets/065970d6-81dc-46dc-9083-6769d4738206)

If you receive an error with your json code, like I did. I utilized Chatgpt to help me write the code and it helped me write the following:
<ul>
    {<br/>
    "title": "Mimikatz Usage Detected"<br/>
    "description": "Mimikatz Detected on host:DESKTOP-69BLBR3\\\\User",<br/>
    "summary": "Mimikatz activity Detected on host: DESKTOP-69BLBR3\\n and the process id is: 2680 and the commandline<br/>           
    is:\\\"C:\\\\Users\\\\User\\\\Downloads\\\\mimikatz_trunk\\\\x64\\\\youareawesome.exe\\\"",<br/>
    "source": "Wazuh",<br/>
    "sourceRef": "Rule: 100002",<br/>
    "type": "external",<br/>
    "status": "New",<br/>
    "severity": 2,<br/>
    "flag": {{ flag }},<br/>
    "pap": 2,<br/>
    "date": 1744366575808,<br/>
    "externallink": "",<br/>
    "tags": ["T1003"]<br/>
    }
</ul>

Paste it into the body of the advanced tab of TheHive Application
![image](https://github.com/user-attachments/assets/6dd59ea1-2e17-4770-9fd7-182bf09f90d4)

And rerun the workflow
![image](https://github.com/user-attachments/assets/79b93843-ce46-4ae3-8ac2-f6161cb4919e)
![image](https://github.com/user-attachments/assets/10b270fb-3dc8-4de5-ae59-01fd2dfe58a4)
![image](https://github.com/user-attachments/assets/e1b18f08-8f95-415d-bd85-f2770e5d7e75)




Log into TheHive Portal as mydfir and you should now see an alert!
![image](https://github.com/user-attachments/assets/b45a3c50-5dec-42a8-bc06-ca9444625fbb)
![image](https://github.com/user-attachments/assets/d7b4282e-bb8c-4f8c-83af-683b297afcb7)


## Step 6: Create an email the contains relevant information to send to a Security Analyst

Head back to our Workflow > Click on Apps > Search for: Email > Download (if you haven’t)> Drag it onto Workflow
![image](https://github.com/user-attachments/assets/13075dc2-056b-4809-8ccd-5c2c78df75c2)


Connect Virus Total to the Email App
![image](https://github.com/user-attachments/assets/2f2b7eee-3cb2-4cbd-bd25-7ca9f34cbb75)

Double Click on the Email App > Create a temporary email address, I used temp-mail.io (if you don’t want to use your own) > paste email in “Recipients” Field > Fill in Subject Field> Fill in Body
![image](https://github.com/user-attachments/assets/c515c147-a97f-43e2-a076-44f86137cf92)

Rerun Workflow and verify if your temp email got the email
![image](https://github.com/user-attachments/assets/3d301c9a-069e-40e2-a2d6-f3ce4a5d3b63)
![image](https://github.com/user-attachments/assets/412128f4-3d86-408c-8aaa-b0a0d1acef17)
![image](https://github.com/user-attachments/assets/44f8ab34-10d9-4f9a-9212-39d1ac51697f)



## Step 7: Active Response Setup

For this section, I will be steering away from MyDFIR’s video and following an amazing resource that I found that revolves around the same project. In myDFIR’s video he mentions to use a Ubuntu VM to  to demonstrate but for my lab environment I wanted to try it with a Windows 10 VM instead.

To configure Wazuh to monitor near real-time changes in the downloads directory we will follow the Wauzh Documentation that Rajat provided us!

### 7a. SSH into the Wazuh Server and open the ossec.conf file
![image](https://github.com/user-attachments/assets/ab1fbd0e-8cc1-44b8-9c6d-d7968a58de54)

### 7b. Finding <syscheck> block and ensuring that <disabled> is set to no
![image](https://github.com/user-attachments/assets/be0c25bc-76ca-4e05-a996-314db558ecb8)

If we scroll down to File Integrity Monitoring section and find the <syscheck> block. We can also verify that the Disabled is set to “no”. this enable the FIM module to monitor directory changes.

### 7c. adding an entry within <syscheck> block to configure a directory to be monitored in near real-time. in our case we configure Wazuh to monitor the Donwloads folder of our Windows 10 VM. We can do this using the following syntax:

    <directories realtime="yes">C:\Users\<USER_NAME>\Downloads</directories>
![image](https://github.com/user-attachments/assets/5e88fc6a-facd-4fc8-87c3-b3171608bee5)

### 7d. Installing Python on our Windows 10 VM
    Login into Windows 10 VM > Install > Run the Python installer
![image](https://github.com/user-attachments/assets/c2bfb697-92f7-49d6-a821-d50fd0178da3)

Once you ran the Installer ensure that Optional Feature “for all users” & Advance options “Add Python 3.X to PATH” is enabled<br/>

Python 3.X to PATH places the interpreter in the execution path
![image](https://github.com/user-attachments/assets/91f14c7e-8441-42b8-a541-406a42d5f865)
![image](https://github.com/user-attachments/assets/6639d624-f9a7-405b-a0c6-ceb10a8386dd)


### 7e. installing Pyinstaller

    Open admin Powershell > command: pip install pyinstaller > to verify we have installed pyinstaller type the command: pyinstaller –version

Pyinstaller is used to convert the active response script that will be added later in an executable application that will run on our Windows endpoint
![image](https://github.com/user-attachments/assets/d597eda5-135a-4c86-ac2e-5134916004d4)

To verify:
![image](https://github.com/user-attachments/assets/d02020dc-c530-499d-91b2-2d30de813f1b)

### 7f. Creating an Active response Script

    Open notepad > Copy and paste  onto the notepad > change “os.path.expander” to your downloads file path > save as “remove-threat.py” file
![image](https://github.com/user-attachments/assets/a96cbfca-d95d-467e-955f-86690aa4a4fd)



Now that we have created the script its time to convert the active response script into a Windows Executable.<br/>

    Open admin Powershell > Change to downloads folder (or wherever you saved your python script > type the command: pyinstaller remove-threat.py
![image](https://github.com/user-attachments/assets/3a6c8ca0-eb69-4a5e-9f82-bb94a3df76da)


Now that we have converted it. Look for the executable and copy it
![image](https://github.com/user-attachments/assets/3739f872-bc17-4dab-a5c7-be848bdef384)

Head over to the Ossec-agent folder > click on active-response > bin > Paste the remove-threat.exe in the folder
![image](https://github.com/user-attachments/assets/86140082-ef20-47ee-9c2d-d1cfa4094dd1)

Head into powershell and restart the service using the command: Restart-Service -Name wazuh
![image](https://github.com/user-attachments/assets/7f19db88-d172-4cab-aed0-32d1227e2a69)

### 7g. Configuring the Wazuh server

    SSH into Wazuh Server > Look for <command> block > Paste the code :

<ul>
    <command>
        <name>remove-threat</name>
        <executable>remove-threat.exe</executable>##Ensure this is your .exe filename
        <timeout_allowed>no</timeout_allowed>
    </command>
    <active-response>
        <disabled>no</disabled>
        <command>remove-threat</command>
        <location>local</location>
        <rules_id>100092</rules_id>
    </active-response>
</ul>
![image](https://github.com/user-attachments/assets/56ab96f4-c560-43e9-9a1b-6224de16d61f)

And then save!

### 7h. Configuring our Shuffle Workflow

Now that we have configured the Active response on our  Wazuh server. We will now modify our shuffle workflow to implement it! The 4 things we will be adding:
<ul>
    A HTTP request icon that will curl for a JWT token from Wazuh on port 55000.<br/>
    A user input icon that will take the user input from the SOC Analyst, with an embedded True and False link in the email.<br/>
    A Wazuh application that will run an active response command.<br/>
    Lastly, an email will inform the SOC Analyst that the Threat has been neutralized.<br/>
</ul>
    
Log into Shuffle and add the “http”  icon to the Workflow and rename it to GET_API. Drag the icon in between the SHA-256 Regex and VirusTotal icons. Once you have placed the icon in between. Connect SHA 256 Regex icon to the GET_API icon and connect the Get_API icon to the Virus total Icon. *See below for reference
![image](https://github.com/user-attachments/assets/f6b7de96-7db0-4824-9f20-4083a570e559)


After you have Done that. Set the find Actions field to Curl. Priro to filling the Statement field. SSH into the Wazuh Server and get wazuh API password. You can find this in the place you extracted the wazuh-install files.tar. You can find the password in the wazuh-passwords.txt. copy the api password onto a notepad as we will need it to write our statement.

Once you have gotten the API Password copy the statement below and paste it into the statement:

    curl -u YourApiUser:YourApiPassword -k -X GET "https://YourWazuhIP:55000/security/user/authenticate?raw=true"

![image](https://github.com/user-attachments/assets/1893c3dd-cbf6-4adf-bb93-e5f0abf48eb6)

The Next step is to get some user input. Drag the user input icon and add the Temporaryt email id to the Email box.
Paste the Text below abd paste it in the Information field:

    Mimikatz activity detected on host: $exec.text.win.system.computer and the processID is: $exec.text.win.system.processID and the command line is: $exec.text.win.eventdata.commandLine
    Choose whether you approve or deny the deletion of the Mimikatz file from the system.

![image](https://github.com/user-attachments/assets/79935209-f12d-468b-b3a2-5667c80317f1)

Next we will now add and configure Wazuh. In Shuffle Install the Wazuh application by clicking on it. After installation, drag the icon onto your workflow.  And connect the User Input to the Wazuh Icon.
![image](https://github.com/user-attachments/assets/082ff937-9500-4f3e-ac04-1103640daa22)

Click on the Wazuh Application and Change the find Actions to “Run Command” >Change the APIkey to GET_API >  Change the URL to your IPaddress:55000
![image](https://github.com/user-attachments/assets/9249bde5-1361-4625-a8d8-2435f3b79ca6)

Choose simple to fille required detail about action. > Change the Command box, type remove-threat (Ensure this matches the name we had in the ossec.conf file) > Change the the Agents list field: select runtime > scroll to agents section and click on id
![image](https://github.com/user-attachments/assets/e36ffdd0-4fcc-4750-b989-0c7cbfdf261a)
![image](https://github.com/user-attachments/assets/218e8fc1-53ad-4cde-8584-f2b4b04f16fb)


Next for our last step of the work flow is to configure an email to let our SOC analyst know that the Threat has been neutralized!

Drag the email icon to our Work flow> Rename the Icon > Connect the Wazuh icon to the Email Icon
![image](https://github.com/user-attachments/assets/b4672296-a99c-4fab-888a-cd4443474971)

Double Click on the Email Icon and configure > Change find Actions field to : Send email shuffle > Change Recipients to your email> Add a Subject > Add a Body Letting the SOC analyst know that the threat has been removed > Save
![image](https://github.com/user-attachments/assets/f184ecf5-68f6-4a42-999c-642364ccda0a)

Here is how our final workflow should look like:
![image](https://github.com/user-attachments/assets/2c52d2e4-4e62-4ef3-ae9c-da01c5efa562)
