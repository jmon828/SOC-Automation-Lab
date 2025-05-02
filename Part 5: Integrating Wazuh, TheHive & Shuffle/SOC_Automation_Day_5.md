## SOC Automation Lab- Day 5
##Objective
Connect Shuffle (SOAR Platform) to send alerts to TheHive and send an alert to SOC Analyst (ME) via Email.
## Skills
A
## Tools
Shuffle
TheHive
VirusTotal
ChatGPT
Temp-mail.io – Temporary mailbox
Pyinstaller – converts script into executable
## Links





## Steps
Step 1: Setting up Shuffle
Head to  site > Create an account > Click on Work flows on the Left-hand side
To create a workflow Click on “Create Workflow button”

You will be prompted to set a Name & Use Case, like below!

You will now be shown a “Change Me” workplace where you can start adding apps and triggers

On the Left hand-Side there is a section called "Triggers” This section includes a couple of Webflow Starters

Drag and Drop Webhook ad Click on it

Once Selected Rename it to Wazuh-Alerts and Copy the Webhook URI – We need to copy it so that we can paste it onto our ossec file hosted on the Wazuh Manager

Prior to Configuring the Wazuh Manager we should verify that the change me icon is set to “Repeat back to me” and ensure that the Call Section is Runtime Argument ($exec) and Save it!


Step 2: Connecting the Wazuh Manager to Shuffle
SSH into Wazuh Manager > nano into ossec file “nano /var/ossec/etc/ossec.conf”

Scroll down after the </global> tag. Copy and paste the integrating tag (can be found ) underneath the <global> tag. – Replace the default <hook_url>  with the webhook URI (the one we copied earlier)

By default, it has a level of three, which means that any alert with a level of three will be sent to Shuffle. We don't want this; instead, we want to change the level to trigger by the rule ID that we configured, 1002, which is our mimikatz alert. - We can do this by replacing the level tag with rule_id

Once were done we will save it and restart the wazuh-manager.service

We can also verify that the service is running by the command systemctl status wazuh-manager.service

Once we have confirmed that the service is active, Next we will regenerate the Mimikatz Telemetry on our Windows 10 VM
Login to Windows 10 VM > Admin Powershell > cd C:\Users\User\Downloads\mimikatz_trunk\x64 > .\youareawesome.exe

Next we want to head to our shuffle instance and Test our workflow
Login to Shuffler > Open your Workflow > Click on Webhook Icon > click Start on the right-hand side

Next Click on the running icon at the bottom and test the workflow

Ensure you have $exec as runtime argument


Inspect the run

Expand the results and you get the results that was generated from Wazuh!

Step 3: Creating a Workflow
Note: the workflow we will be creating will be designed around this demo. But the same concepts can be applied to other use cases!
Workflow Summary:
Mimikaztz Alert sent to Shuffle
Shuffle will receive the Mimikatz Alert – Extract SHA256 Hhash from File
Check the Reputation score w/VirusTotal
Send Details to TheHive to Create Alert
Send Email to SOC Analyst to Begin Investigation
When we look at the return values for the hashes we can see that it is appended by their hash type

If we wanted to automate this we would need to parse out the hash value itself because if we don’t do that step the entire value including the “SHA1=” will be sent to VirusTotal to get a reputation score. We don’t want that, we just want the Hash value itself. To do this we must:
Go into Shuffle > Click on Change me icon > Rename it to SHA-256 > Change Repeat back to me to “Regex capture group” >

Input Data: Hashes > utilize chat GPT to create a Regex to parse the SHA256 hash > Copy and Paste the Created regex into the Regex Box > Save




Once Saved Click on the Workflow run icon > Rerun the workflow


Once Rerun is done, we can scroll to the SHA-256 result, expand it and notice that it parsed out the SHA256 hash!

Step 4: Setting up  API to check reputation score
Prerequisite: Create a VirusTotal account
4a. Copy VirusTotal Key


4b. Head back to shuffle and add the Virus Total App

Once the app is Activated > Drag and drop the App on to our workflow

4c. Configure VirusTotal App

Click on Authenticate Virustotal V3 to apply the API key you copied earlier and submit\


Once Submitted > Ensure that the”ID” is set to the Regex output. Remember the regex is there to parse out the value of the hash!

4d. VirusTotal Testing and Troubleshooting
Once you have saved the workflow click on the run icon to show previous workflows

Click on a recent workflow and rerun


Once the Rerun is finished, expand the Virustotal window


If you have no hash at the end of the “url” section – follow the video at 15:50
Once you have done that and re-ran the work flow you should see the results below

Analyzing Further if we expand the Attributes Section

Then scroll to the “last_analysis_stats” Section

We can see that there is a number 64 items for the malicious field

This means that there were 64 scanners that marked this as malicious

Step 5: Implementing theHIVE
5a. Add TheHive application to our workflow












5b. configuring TheHive Server
Head to TheHive website & login

If you are getting any sort of authentication error, this usually means that elasticsearch isn’t running. You might have to remote into it and start the service again. – this was done in Day 3

5c. adding another organization and creating a new user under that organization
By default we only have one organization. We can add another by clicking the plus icon on the top left

Next we fill in the Name and Description fields and confirm

Once that is done click on it and immediately we are greeted with a “no users have been found” screen


To add a user click on the “+” icon

When prompted we can now create a user

Next I will create another account but this time it will be used as a service account

At the end the Organization should have two users:

5d. Applying passwords to our users
Hover over the mydfir account and select preview

Scroll down and click on “set password” – Add a password and confirm


5e. Creating an API Key for the SOAR user (admin)
Hover over the SOAR user and click on preview

Scroll a bit to “API Key” section and click on create

Copy and paste the key onto a notepad as we will be using it later on!
5f. Verifying user accounts work
Login using the username and password that we created

No cases have been found should be the default
5g. Setting up TheHive in our Workflow
Log back into shuffle and double click on TheHive app that we placed on our workflow

Click on Authenticate TheHive

Paste the API Key that we copied earlier & for the URL, paste the public IP of your TheHive Instance along with the port number – Then hit Submit

Now under the find actions we want to “create an alert” – click on the drop down, and select Create Alert

Now we scroll a bit to fill in the fields.
When following MYDFIR’s video I came across an issue where I didn’t have the same field as him. To fix this, I added another line to the JSON code to add the field I was missing. I did this by:
Head to TheHive App > Clicking on Create Alert action > Followed the JSON format and add the missing field – in this case, I was missing the “date” field therefore I added the “date”: “${date}” JSON line (See Below) > Then Submit.


Head back into our Workflow and we can now see the new “Date” field

Now, Selecting the date we can now add a Runtime argument
Click on the “+” icon beside the Field

Hover over Runtime Argument and Select utcTime

Next do the Following:
For description, Type the following:
Mimikatz Detected on host:$exec.text.win.system.computer from user: $exec.text.win.eventdata.user


pap = permissible actions protocol (level of exposure of information)
Source = where the alert is coming from
Sourceref= metadata for rule
Status= status of the alert that will be created in TheHive
Summary = Similar to Description
TLP = Traffic light protocol = confidentiality of information
Type = where did the alert come from
5h. Configuring Cloud firewall to allow all IP’s coming inbound on port 9000 (where our Hive Instance will live.
Login to Digital Ocean > Networking > Firewalls > Select the firewall that we created

Click on new rule > and click custom

> Change the port to 9000 > Remove All IPv6 > save



5i. Testing and Verifying
Go back into TheHive > Click on the “Running Man icon” > Rerun workflow

If you receive an error with your json code, like I did. I utilized Chatgpt to help me write the code and it helped me write the following:
{
"title": "Mimikatz Usage Detected",
"description": "Mimikatz Detected on host:DESKTOP-69BLBR3\\\\User",
"summary": "Mimikatz activity Detected on host: DESKTOP-69BLBR3\\n and the process id is: 2680 and the commandline is:\\\"C:\\\\Users\\\\User\\\\Downloads\\\\mimikatz_trunk\\\\x64\\\\youareawesome.exe\\\"",
"source": "Wazuh",
"sourceRef": "Rule: 100002",
"type": "external",
"status": "New",
"severity": 2,
"flag": {{ flag }},
"pap": 2,
"date": 1744366575808,
"externallink": "",
"tags": ["T1003"]
}
Paste it into the body of the advanced tab of TheHive Application

And rerun the workflow




Log into TheHive Portal as mydfir and you should now see an alert!


Step 6: Create an email the contains relevant information to send to a Security Analyst

Head back to our Workflow > Click on Apps > Search for: Email > Download (if you haven’t)> Drag it onto Workflow


Connect Virus Total to the Email App

Double Click on the Email App > Create a temporary email address, I used temp-mail.io (if you don’t want to use your own) > paste email in “Recipients” Field > Fill in Subject Field> Fill in Body

Rerun Workflow and verify if your temp email got the email



Step 7: Active Response Setup

For this section, I will be steering away from MyDFIR’s video and following an amazing resource that I found that revolves around the same project. In myDFIR’s video he mentions to use a Ubuntu VM to  to demonstrate but for my lab environment I wanted to try it with a Windows 10 VM instead.

To configure Wazuh to monitor near real-time changes in the downloads directory we will follow the Wauzh Documentation that Rajat provided us!

7a. SSH into the Wazuh Server and open the ossec.conf file

7b. Finding <syscheck> block and ensuring that <disabled> is set to no

If we scroll down to File Integrity Monitoring section and find the <syscheck> block. We can also verify that the Disabled is set to “no”. this enable the FIM module to monitor directory changes.

7c. adding an entry within <syscheck> block to configure a directory to be monitored in near real-time. in our case we configure Wazuh to monitor the Donwloads folder of our Windows 10 VM. We can do this using the following syntax:
<directories realtime="yes">C:\Users\<USER_NAME>\Downloads</directories>

7d. Installing Python on our Windows 10 VM
Login into Windows 10 VM > Install > Run the Python installer

Once you ran the Installer ensure that Optional Feature “for all users” & Advance options “Add Python 3.X to PATH” is enabled
Python 3.X to PATH places the interpreter in the execution path







7e. installing Pyinstaller
Open admin Powershell > command: pip install pyinstaller > to verify we have installed pyinstaller type the command: pyinstaller –version

Pyinstaller is used to convert the active response script that will be added later in an executable application that will run on our Windows endpoint

To verify:

7f. Creating an Active response Script

Open notepad > Copy and paste  onto the notepad > change “os.path.expander” to your downloads file path > save as “remove-threat.py” file

Now that we have created the script its time to convert the active response script into a Windows Executable.
Open admin Powershell > Change to downloads folder (or wherever you saved your python script > type the command: pyinstaller remove-threat.py


Now that we have converted it. Look for the executable and copy it

Head over to the Ossec-agent folder > click on active-response > bin > Paste the remove-threat.exe in the folder

Head into powershell and restart the service using the command: Restart-Service -Name wazuh

7g. Configuring the Wazuh server
SSH into Wazuh Server > Look for <command> block > Paste the code :
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


And then save!

7H. Configuring our Shuffle Workflow

Now that we have configured the Active response on our  Wazuh server. We will now modify our shuffle workflow to implement it! The 4 things we will be adding:
A HTTP request icon that will curl for a JWT token from Wazuh on port 55000.
A user input icon that will take the user input from the SOC Analyst, with an embedded True and False link in the email.
A Wazuh application that will run an active response command.
Lastly, an email will inform the SOC Analyst that the Threat has been neutralized.

Log into Shuffle and add the “http”  icon to the Workflow and rename it to GET_API. Drag the icon in between the SHA-256 Regex and VirusTotal icons. Once you have placed the icon in between. Connect SHA 256 Regex icon to the GET_API icon and connect the Get_API icon to the Virus total Icon. *See below for reference


After you have Done that. Set the find Actions field to Curl. Priro to filling the Statement field. SSH into the Wazuh Server and get wazuh API password. You can find this in the place you extracted the wazuh-install files.tar. You can find the password in the wazuh-passwords.txt. copy the api password onto a notepad as we will need it to write our statement.

Once you have gotten the API Password copy the statement below and paste it into the statement:
curl -u YourApiUser:YourApiPassword -k -X GET "https://YourWazuhIP:55000/security/user/authenticate?raw=true"

The Next step is to get some user input. Drag the user input icon and add the Temporaryt email id to the Email box.
Paste the Text below abd paste it in the Information field:
Mimikatz activity detected on host: $exec.text.win.system.computer and the processID is: $exec.text.win.system.processID and the command line is: $exec.text.win.eventdata.commandLine

Choose whether you approve or deny the deletion of the Mimikatz file from the system.

Next we will now add and configure Wazuh. In Shuffle Install the Wazuh application by clicking on it. After installation, drag the icon onto your workflow.  And connect the User Input to the Wazuh Icon.

Click on the Wazuh Application and Change the find Actions to “Run Command” >Change the APIkey to GET_API >  Change the URL to your IPaddress:55000

Choose simple to fille required detail about action. > Change the Command box, type remove-threat (Ensure this matches the name we had in the ossec.conf file) > Change the the Agents list field: select runtime > scroll to agents section and click on id


Next for our last step of the work flow is to configure an email to let our SOC analyst know that the Threat has been neutralized!

Drag the email icon to our Work flow> Rename the Icon > Connect the Wazuh icon to the Email Icon

Double Click on the Email Icon and configure > Change find Actions field to : Send email shuffle > Change Recipients to your email> Add a Subject > Add a Body Letting the SOC analyst know that the threat has been removed > Save

Here is how our final workflow should look like:

Step 8:Troubleshooting
