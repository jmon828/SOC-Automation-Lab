## Objective: 
Using Steven’s (MyDFIR) guide - Install Applications and Setup Virtual Machines

## Skills
1.	Learn the basics of installing & Configuring a Virtual Machine on a Type 2 Hypervisor 
2.	Learned how to Install  Sysmon tool 
3.	Learned how to Install TheHive and Wazuh via SSH
4.	Utilize a Cloud provider (Digital Ocean) to createa Virtual Machine

## Tools:
<ul>
<l1>•	Sysmon – free Microsoft tool part of the sys internal suite – it provides you with a lot of telemetry, allowing you to increase the chances of you </l1><br/>
<l2>•	Hive- 4-in-1 Open-source Security incident response platform</l2><br/>
<l3>•	Wazuh- open source cybersecurity platform that integrates SIEM and XDR capabailities </l3><br/>
<l4>•	VirtualBox – </l4><br/>
<l5>•	Digital Ocean- Cloud Platform </l5><br/>
<l6><a>• </a><a href="https://www.putty.org/">PuTTY </a><a>SSH terminal emulator </l6><br/>
</ul>

## Steps
Step 1: Install VirtualBox<br/>
![image](https://github.com/user-attachments/assets/facb7ec4-2f4a-4458-9180-49c3f98fe3ed)<br/>
head to https://www.virtualbox.org/wiki/Downloads and choose the installation according to your operating system. (I chose the Windows hosts)
Verify Checksum (available on VirtualBox’s Website) and Run the installer
![image](https://github.com/user-attachments/assets/56bcb09c-58c2-4b30-966f-91aa530554e7)
Once you have ran the installer. Keep clicking "Next" or "Yes", Changing the location of the installation Accordingly
![image](https://github.com/user-attachments/assets/405e0e6e-7264-40ff-96a7-9542b2e88290)
![image](https://github.com/user-attachments/assets/62bc7fcf-6f54-4357-b3b2-72362aa41cc8)<br/>
Install Dependencies, Select the way you want the features to be installed, and Install the program!
![image](https://github.com/user-attachments/assets/7dbaf2e6-8a03-45ee-b593-a134b28e6222)
![image](https://github.com/user-attachments/assets/eba696da-5268-44e7-a93d-c30101391117)
![image](https://github.com/user-attachments/assets/2d25a123-049a-4ae3-bb8c-1e75e4838f5c)<br/>
Note: You might have to restart you Computer before opening VirtualBox<br/>

Step 2: Install and Configure Windows 10<br/>
![image](https://github.com/user-attachments/assets/56632ebb-730e-4f17-b017-f060776270ad)<br/>
Head over to https://www.microsoft.com/en-us/software-download/windows10 and download a Media Creation file to generate a Windows 10.iso file<br/>

You would then be greeted with a License agreement screen, in which you would hit Accept.

Once you have accepted the License you are then directed to this page. – this screen Defaults to Upgrade this PC now” so in order to create a .iso file we would have to switch it over to the “Create Installation media (USB flash drive, DVD, or ISO file) for another PC” and hit next!
![image](https://github.com/user-attachments/assets/2534ea37-bd15-4d6e-94db-55c43767a227)
The Next Step, prompts you to select your desired Language, Architecture and Edition
![image](https://github.com/user-attachments/assets/d4ce08a5-7cca-4f5c-86d1-808ea993921c)
Next You are then presented with the option to choose which media (USB) to use. – choose the ISO file and save it to your desired location
![image](https://github.com/user-attachments/assets/332f33e8-dc9c-4051-821f-9c0a95a80294)

Step 3: Integrating Windows 10 with Virtual Box<br/>
Continuing after the previous step, Open VirtualBox > Click on machine (top Left) and click new
![image](https://github.com/user-attachments/assets/92a5c79b-6fc6-46ba-9b93-936f5f7b04a5)
You are then Presented with the screen below
![image](https://github.com/user-attachments/assets/608cba61-e46c-46fb-a383-531efbd89e13)
There are three categories that need to be filled:
<ul>
<l1>o	Name and Operating System</l1><br/>
<l2>o	Hardware</l2><br/>
<l3>o	Hard disk</l3><br/>
</ul>

For Name and Operating System- Fill properties Accordingly *See Below for Example:
<ul>
<l1>1.	Name your Virtual Machine</l1><br/>
<l2>2.	Change “Folder” property to where you want to install</l2><br/>
<l3>3.	Ensure the “ISO Image” property is correct<l3><br/>
<l4>4.	Ensure that “Skip Unattended Installation is Checked</l4><br/>
</ul>
 
![image](https://github.com/user-attachments/assets/cc823177-73f5-4b58-ac43-7ad0035dfaf9)

Hardware – Fill Properties according to what your physical system can handle *See Below for Example
![image](https://github.com/user-attachments/assets/604f045c-4693-47f5-b7a7-58268e12e2c1)

Hard Disk – this is how large the Virtual Disk is – See below for Example
![image](https://github.com/user-attachments/assets/1f07ada1-d6ba-4f36-9eb0-03a3874a847b)
*Hit Finish After!

Step 4: Windows 10 Configuration<br/>
Once your VM is done you can start your VM
![image](https://github.com/user-attachments/assets/6fa4349a-c46d-45c2-bc89-abfe5d7903b7)
Once bootup is done > Setup Windows 10>
![image](https://github.com/user-attachments/assets/2f25aed8-8875-45a7-804e-e431f3fb4490)
![image](https://github.com/user-attachments/assets/8f0db47b-12eb-475e-800b-c8ba9f2e190d)

 
Once that screen is by passed > Click “I don’t have a private key”
![image](https://github.com/user-attachments/assets/d8050f81-6142-46be-95a6-de08adf434d3)

Select Windows 10 Pro & Accept the Terms
![image](https://github.com/user-attachments/assets/357010bf-cea2-448e-a0f2-85bb792e18aa)
![image](https://github.com/user-attachments/assets/cb7564ec-e170-4c46-bc37-b346a0446c08)

 
*Select Custom Windows Only > Hit Next > Let Windows Install
 ![image](https://github.com/user-attachments/assets/46d6b08c-21d4-4e19-a9ae-1c2df3f78946)

Select Where you want to install Windows (I selected the Default)
![image](https://github.com/user-attachments/assets/155628b2-508d-486a-8207-f080bb8f3dd5)

 
 
*Once done install a web browser. Also, take a snapshot of your VM for best practices
![image](https://github.com/user-attachments/assets/50d602e7-db2e-4e31-b4bc-b8e31a8b566d)

Step 5: Installing Sysmon
Download Sysmon on you computer <a href=https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)>Sysmon Link</a>
![image](https://github.com/user-attachments/assets/a00d9ee6-3f7c-4f9e-bfb2-b3ba442babba)

While Waiting you can the Sysmon config file (https://raw.githubusercontent.com/olafhartong/sysmon-modular/refs/heads/master/sysmonconfig.xml) >Right-click >  Save as “sysconfig”
![image](https://github.com/user-attachments/assets/291ab8d5-7c13-4dc8-857a-9e763a4613e2)

Once the Sysmon tool is done installing > Right-click > Extract .zip file to desired location
![image](https://github.com/user-attachments/assets/e8bea301-e583-481c-b845-b8f83eac65a6)
![image](https://github.com/user-attachments/assets/a0f2c34d-a6f4-475b-87bc-cacf20e2d1fe)

Open Powershell (Administrative) > 
![image](https://github.com/user-attachments/assets/e2a340a2-b2da-4201-840b-3b5c294c09c5)

Change directories to where you extracted the Sysmon .zip file > use “dir” command to see files in the folder
![image](https://github.com/user-attachments/assets/644940e3-d292-468d-8b34-15af1d89213a)


Run the “Sysmon64.exe” using the command .\Sysmon64.exe – Once you press enter you should get a Manual page. 
![image](https://github.com/user-attachments/assets/5cf920ee-8028-4770-87f1-141747aa2cb5)

To verify that Sysmon is installed: 
	Services > Manually look for Sysmon – if it’s not there (like mine) – go to the next step!

 ![image](https://github.com/user-attachments/assets/9af17567-51cc-4aae-931c-6a23131aca50)

 Head over to Power shell and run the “.\Sysmon64.” Once again but this time run:
	 “.\Sysmon64 -i sysmonconfig.xml”
Ensure  that the “sysmonconfig.xml” Is in the same directory as “sysmon64.exe”

![image](https://github.com/user-attachments/assets/cc472e3c-93e1-4ac3-bb5e-3b146e5bd740)
![image](https://github.com/user-attachments/assets/e4e1a86b-d1c5-4c34-b919-5e09bbedaf2f)

Re-check Service again to verify Sysmon is installed: Windows Key + R > Services.msc > look for Sysmon64
![image](https://github.com/user-attachments/assets/210ec869-c24d-461a-b752-9409e8129f2d)

Step 6: Installing Wazuh<br/>
Disclaimer: There are other ways of doing this – My HOST Computer does not have enough resources for me to host another VM therefore I followed the video (in resources) to host the Wazuh client on the cloud. <br/>

On your HOST Computer head to Digital Ocean > Create your Account > Create > Droplets (Droplets are VMs)
![image](https://github.com/user-attachments/assets/d12bf7ae-f251-4a93-9a95-659e2394ccd3)

Create Droplets > Choose Region (Choose the region that is appropriate for you) > 
![image](https://github.com/user-attachments/assets/a8d67edc-7788-4721-b494-252db0b499a8)


Choose an Image > Ubuntu > Version: 24.04 LTS x64

![image](https://github.com/user-attachments/assets/161e3813-8bc3-41c4-8cc6-ac15d3dff79e)

Choose Size > Basic > Premium Intel > 48.00/mo (This is covered with the link provided from the guide)
*Ensure that our specs have at least 8GBs of RAM & 50 GB Hard Drive Space

![image](https://github.com/user-attachments/assets/673a8e67-b7fc-4a99-a7df-eec6e7504f1a)

Scroll down to Choose Authentication Method > Password >
	*Utilize a Password manager as we will be exposing this to the internet and therefore the password should be strong

![image](https://github.com/user-attachments/assets/b0795b7f-41eb-4594-a7e2-49c5cba2ca71)
Scroll to Finalize Details > Hostname: Wazuh (or any name) > Create Droplet 

![image](https://github.com/user-attachments/assets/5f6802ba-69e1-455f-aeb1-9a6ee1009838)
![image](https://github.com/user-attachments/assets/f156ddee-6b4e-4251-8074-cf51d1daa53f)

While Waiting we will setup a Firewall 
	Left Hand-side> Drop down Manage> Networking 
 ![image](https://github.com/user-attachments/assets/8b208cc6-7c2f-4614-b6ad-4193b8f26958)

Firewalls Tab >Create Firewall
![image](https://github.com/user-attachments/assets/72881dd5-8e0e-43f0-863a-00a62bbd6da2)


Name : Firewall (name can be anything) > Inbound Rules: Type – All TCP > Inbound Rules:  Sources – Your public IP > 
	*Your Public IP can be found: whatismyipaddress.com 
	*This is done because, if we do not make rules, our “Wazuh” VM will be accessible to anyone on the internet.  18:00 mins *Review 

![image](https://github.com/user-attachments/assets/430d9ba4-9200-48e6-8ec0-dc75796f7413)
Scroll down > Create Firewall
![image](https://github.com/user-attachments/assets/3747140a-0594-4639-94b2-f00a8c879354)

Check on our Wazuh Server > Networking > Firewalls: Edit>  Select Firewall > Droplets tab > Add Droplets > Select Wazuh > Add 
![image](https://github.com/user-attachments/assets/713d14c9-1dac-4b51-a124-18eb264f0d61)
![image](https://github.com/user-attachments/assets/3a68d3bd-9bbf-483c-94d0-45cfaacf206d)
![image](https://github.com/user-attachments/assets/28cc1e81-557a-4185-8f0e-0300f3fdd6f9)
![image](https://github.com/user-attachments/assets/3e456a25-6cfd-4df3-95ff-9ae4a0fc43e7)
![image](https://github.com/user-attachments/assets/be2d73c8-a26c-429f-84f0-6615cc997d00)
![image](https://github.com/user-attachments/assets/3f30ab4d-3801-4974-a330-db5fb8942fee)
![image](https://github.com/user-attachments/assets/8d3ea3f2-ebed-4fbf-83ac-e3d0dc2ae3e6)

Access the Wazuh Server > Launch console 
	*you might be “not authorized” to launch therefore it used PuTTy instead
![image](https://github.com/user-attachments/assets/52fe531f-d429-4bfc-ad61-67c2354ca691)

Since I had PuTTy installed from a previous project I used that instead – if you are using PuTTy Follow below:
	Copy Wazuh Server ipv4 address > Open PuTTY > Host Name (or IP address): “Wazuh Server IPv4” > Accept Prompt >login as “root” > Enter password of Wazuh server
![image](https://github.com/user-attachments/assets/88615ec2-a85f-4496-a511-26416bb3812b)
![image](https://github.com/user-attachments/assets/c44e0860-d200-4f74-9b3f-3dc9076af7be)
![image](https://github.com/user-attachments/assets/51614b51-f90e-4848-af1f-af234b3887a6)
![image](https://github.com/user-attachments/assets/0734bced-ee66-48e6-9998-54a7a3f97d9a)
![image](https://github.com/user-attachments/assets/db97835d-bc69-4fa4-bc64-19f771360986)

Once we have successfully SSHed into our VM we can start performing some updates and upgrades:
	Under Root user > Command “apt-get update && apt-get upgrade -y” 

![image](https://github.com/user-attachments/assets/7874deaf-2c51-4770-96f9-16c97a8302c2)
Once Done we can now start Installing Wazuh, we can do this by run a curl command found on their website:
	Root user > curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh && sudo bash ./wazuh-install.sh -a -i 
![image](https://github.com/user-attachments/assets/b0b4ae3a-4999-4594-94de-0a80954193fc)
Once finished. Save the Admin Credentials for your Wazuh Platform

![image](https://github.com/user-attachments/assets/2fa853bf-4880-40e4-a91f-58bec57025d6)
Open a web browser > Type: https://167.99.189.219 in URL Bar > Advanced > Proceed to 167.99.189.219 (unsafe) > Enter Credentials
![image](https://github.com/user-attachments/assets/22c9020d-6dce-458a-b2a2-1e7161e2bc34)
![image](https://github.com/user-attachments/assets/31ee507f-de96-4418-9949-f93adec848cc)
![image](https://github.com/user-attachments/assets/ec246d60-c604-404a-a910-8e38896c2606)

Step 7: Installing TheHive
	*Disclaimer this server will also be on Digital Ocean
Same as Before Create Droplets > Choose Region > Choose Image Ubuntu > Specs Similar to Wazuh > Password > Hostname 
![image](https://github.com/user-attachments/assets/34b026e6-8a51-4648-ae60-bfd0e07f0e7d)
![image](https://github.com/user-attachments/assets/0f31c989-f614-4283-8221-f479b2a0160e)
![image](https://github.com/user-attachments/assets/15153d7d-d5f1-4c76-8b9f-9114d3be096c)
![image](https://github.com/user-attachments/assets/d8f52729-5b6c-456f-9bb2-4258868d4fc6)
Once created Configure firewall
	Click on The Hive > Networking > Firewalls: Edit> Select the Firewall that we made earlier > Droplets tab > Add Droplets > Add The Hive
![image](https://github.com/user-attachments/assets/7b59ecf1-e914-40e0-9e74-caa21c6a3cbe)
![image](https://github.com/user-attachments/assets/8cbc977d-2fa5-45ab-b518-6080135143b7)
![image](https://github.com/user-attachments/assets/d56e0c36-75e9-4029-b065-441d61737fa4)
![image](https://github.com/user-attachments/assets/bd07f6e4-a62b-4d90-9f14-8175f54405ba)

Once the Firewall has been created Go ahead and SSH into the The Hive using puTTY (or any other SSH tool)
	Copy ipv4 > Open PuTTY > Enter ipv4 address > open > Accept Prompt > Login as root user
![image](https://github.com/user-attachments/assets/693b5773-2bb5-4742-90b2-0fcf4118b46b)

![image](https://github.com/user-attachments/assets/42b463fd-7e5b-48f6-9076-49fccc9e7535)
![image](https://github.com/user-attachments/assets/a5b5f6c5-a3f7-40a5-8dd2-adf4bdc3b3e6)
Once you are in, we must install Java, Cassandra, Elastic Search, The Hive 
As root user > install Dependencies Command: apt install wget gnupg apt-transport-https git ca-certificates ca-certificates-java curl  software-properties-common python3-pip lsb-release
![image](https://github.com/user-attachments/assets/25af4c47-561d-4d6a-a142-9122fb75eab5)


Once finished, we can install JAVA<br/>
As Root user > Copy & Paste the commands line by line below:
1.	wget -qO- https://apt.corretto.aws/corretto.key | sudo gpg --dearmor  -o /usr/share/keyrings/corretto.gpg
2.	echo "deb [signed-by=/usr/share/keyrings/corretto.gpg] https://apt.corretto.aws stable main" |  sudo tee -a /etc/apt/sources.list.d/corretto.sources.list
3.	sudo apt update
4.	sudo apt install java-common java-11-amazon-corretto-jdk
5.	echo JAVA_HOME="/usr/lib/jvm/java-11-amazon-corretto" | sudo tee -a /etc/environment 
6.	export JAVA_HOME="/usr/lib/jvm/java-11-amazon-corretto"<br/>

Once finished, We can now install Cassandra
	As Root user > Copy & Paste the commands line by line below:
1.	wget -qO -  https://downloads.apache.org/cassandra/KEYS | sudo gpg --dearmor  -o /usr/share/keyrings/cassandra-archive.gpg
2.	echo "deb [signed-by=/usr/share/keyrings/cassandra-archive.gpg] https://debian.cassandra.apache.org 40x main" |  sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list
3.	sudo apt update
4.	sudo apt install Cassandra<br/>

Once Cassandra is finished we can install Elastic Search<br/>
As Root user > Copy & Paste the commands line by line below:<br/>
1.	wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch |  sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
2.	sudo apt-get install apt-transport-https
3.	echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main" |  sudo tee /etc/apt/sources.list.d/elastic-7.x.list
4.	sudo apt update
5.	sudo apt install elasticsearch<br/>

Once Elastic Search is Done, we can now install TheHive<br/>
As Root user > Copy & Paste the commands line by line below:
1.	wget -O- https://archives.strangebee.com/keys/strangebee.gpg | sudo gpg --dearmor -o /usr/share/keyrings/strangebee-archive-keyring.gpg
2.	echo 'deb [signed-by=/usr/share/keyrings/strangebee-archive-keyring.gpg] https://deb.strangebee.com thehive-5.2 main' | sudo tee -a /etc/apt/sources.list.d/strangebee.list
3.	sudo apt-get update
4.	sudo apt-get install -y thehive
Once everything is finished Wecan now Configure Wazuh and The Hive

