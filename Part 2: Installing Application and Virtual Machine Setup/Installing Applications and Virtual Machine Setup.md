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


