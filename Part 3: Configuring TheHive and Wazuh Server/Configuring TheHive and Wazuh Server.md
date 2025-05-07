# Objective:
Configure TheHive and Wazuh Servers up and running properly & have a Windows 10 client reporting into the Wazuh server.

# Skills
<ul>
	<l1>1.	Learned how to configure TheHive</l1>
	<l2>2.	Learned how to configure a Wazuh server</l2>
	<l3>3.	Configured a Windows 10 Machine to report to Wazuh</l3>
</ul>

# Tools/Commands
<ul>
<l1>Cassandra – Used for theHive’s database</l1><br/>
<l2>Elastic Search – used to mangae data indices (querying data)</l2><br/>
<l3>Nano – linux text editor </l3><br/>
</ul>

# Links 
<ul>
	<a href="https://www.youtube.com/watch?v=VuSKMPRXN1M&list=PLYHfX1HJ8dv8RVatf6ULT1Ga5RaLMWreQ&index=8&t=6s">SOC Automation Project - MyDFIR</a>
</ul>

# Steps 
<ul>
	<a href ="https://github.com/jmon828/SOC-Automation-Lab/blob/main/Part%203:%20Configuring%20TheHive%20and%20Wazuh%20Server/Configuring%20TheHive%20and%20Wazuh%20Server.md#step-1-configure-thehive">Step 1: Configure TheHIVE</a><br/>
	<ul>
		<l1 href="https://github.com/jmon828/SOC-Automation-Lab/blob/main/Part%203:%20Configuring%20TheHive%20and%20Wazuh%20Server/Configuring%20TheHive%20and%20Wazuh%20Server.md#1a-configuring-cassandra">1a. Configuring Cassandra</l1><br/>
		<l2 href="https://github.com/jmon828/SOC-Automation-Lab/blob/main/Part%203:%20Configuring%20TheHive%20and%20Wazuh%20Server/Configuring%20TheHive%20and%20Wazuh%20Server.md#1b-configure-elastic-search--open-up-the-elasticsearch-config-file">1b. Configure Elastic Search – Open up the elasticsearch config file</l2><br/>
		<l3 href = "https://github.com/jmon828/SOC-Automation-Lab/blob/main/Part%203:%20Configuring%20TheHive%20and%20Wazuh%20Server/Configuring%20TheHive%20and%20Wazuh%20Server.md#1c-finalizing-thehive-">1c: Finalizing TheHive. </l3><br/>
		<l4 href ="" >1d. Verifying Functionality of TheHive </l4><br/>
	</ul>
	<a href ="https://github.com/jmon828/SOC-Automation-Lab/blob/main/Part%203:%20Configuring%20TheHive%20and%20Wazuh%20Server/Configuring%20TheHive%20and%20Wazuh%20Server.md#step-2-configuring-wazuh-">Step 2: Configuring the Wazuh Server</a>
</ul>

## Step 1: Configure TheHIVE<br/>

### 1a. Configuring Cassandra 
PuTTY (or SSH) into HIVE Server

	Copy public IP > Open PuTTY > Paste ip > Login
![image](https://github.com/user-attachments/assets/adc36c69-859a-4d9d-aeb1-aab0c240fad7)
![image](https://github.com/user-attachments/assets/e49c8353-6c41-4d99-8c3e-952c9142f179)

From the previous day, we installed multiple components. One of those components was Cassandra.<br/>

	Edit Cassandra’s config file: Root> nano /etc/cassandra/Cassandra.yaml
![image](https://github.com/user-attachments/assets/4fbec270-ba2d-45ea-98ef-448063cdd990)
![image](https://github.com/user-attachments/assets/3405923e-fd89-4056-9403-280b6c755d72)

From here we will customize our listen address or ports along with the cluster name.<br/>

	Use arrow keys to change “Cluster_name” – you can change this to what ever name you want!
![image](https://github.com/user-attachments/assets/c7618fde-3de2-44aa-a9f5-2c568fef0949)

Next we want to find the listen address and RPC address<br/>

	Hold CTRL + W > search for listen > change “listen_address” (Enter Public IP of THE Hive)
![image](https://github.com/user-attachments/assets/1cd0a798-1db8-4a30-a02c-4de7eb64442d)

Next we want to find RPC address<br/>

	Again Hold CTRL+W > Search for “RPC” > change “rpc_address” (Enter Public IP of The HIVe)
![image](https://github.com/user-attachments/assets/f86ba35a-07e9-41b5-9726-be246e8e09c6)

Next we must change the Seed address<br/>

	Again Hold CTRL+W > Search for “seed” > change “seeds” (Enter Public IP of The Hive – keep the port number do not change it) > CTL + X > Y (This saves and exits the nano editor)
![image](https://github.com/user-attachments/assets/58231d78-bb75-4e2a-a04a-0ef652f7fc20)

Next we will stop the Cassandra service<br/>

	Root > systemctl stop Cassandra.service
![image](https://github.com/user-attachments/assets/030b390d-6be9-43fc-8f48-c81fdf334877)

Since we installed TheHive using their package, we will now remove old files<br/>

	Root > rm -rf /var/lib/Cassandra/*
![image](https://github.com/user-attachments/assets/312db691-0a75-48c3-85ce-71f6a70dfd1a)

Now that the old files have been remove, we can start (Cassandra) it back up again<br/>

	Root > systemctl start Cassandra.service
![image](https://github.com/user-attachments/assets/c6899c8d-0619-4677-bafc-1aca67872e8b)

Verify that Cassandra is running<br/>

	Root > systemctl status Cassandra.service
![image](https://github.com/user-attachments/assets/73aab443-2151-45a8-a6d6-b65e6ea6c663)
_______________________________________________________________________________________________________________________
### 1b. Configure Elastic Search – Open up the elasticsearch config file<br/>

	Root > nano /etc/elasticsearch/elasticsearch.yml
![image](https://github.com/user-attachments/assets/90aa04e3-4d98-4698-8111-3af1532abbc7)
![image](https://github.com/user-attachments/assets/a02b5be1-bdad-4524-bac7-8f48b85b2188)

Here we will change Cluster name, node.name, network.host<br/>

	Scroll down to cluster.name > delete the comment (“#”) > Change the name to “thehive”
![image](https://github.com/user-attachments/assets/038d85a5-be52-4e52-88d3-6d77c3d8d4aa)

	Scroll down to node.name > Delete the comment (“#”) > Leave as is
![image](https://github.com/user-attachments/assets/4e0514ad-c4bf-4a96-b0cb-9d3cf051649f)

	Scroll do to network host > Delete the comment (“#”) > change ip to public ip of the hive
![image](https://github.com/user-attachments/assets/208339a4-4e3b-4e2b-a595-d3901bdfa475)

	Scroll down to http port > Delete the comment (“#”) > leave as is
![image](https://github.com/user-attachments/assets/323db8cc-fc14-44ef-9c00-fba635b704a5)

	Scroll down to cluster.initial_master_nodes > Delete the comment (“#”) > delete “, node -2” – since I only have one node > CTRL + X > Y – to save
![image](https://github.com/user-attachments/assets/5116dc78-b44e-43d2-bc94-02912c7da784)

Start the elasticsearch service 

	Root > systemctl start elasticsearch > systemctl enable elastic search > systemctl status elastic search (to verify)
![image](https://github.com/user-attachments/assets/b95f8761-3418-4c25-b879-64c8ada5410b)

_______________________________________________________________________________________________________________________________________
### 1c: Finalizing TheHive. <br/>
before we finalize we want to make sure that TheHives User & groups have access to a certain file path.

	Root > ls -la /opt/thp
![image](https://github.com/user-attachments/assets/ba0d8455-4c1f-4fe4-bd32-21efdd46fc92)
![image](https://github.com/user-attachments/assets/9ae239fb-22ad-4c80-abdf-f63c79a75e18)

As you can see root user has access to “thehive” to change that we must: <br/>

	Root > chown  -R thehive:thehive /opt/thp > enter
 ![image](https://github.com/user-attachments/assets/de4c1b85-8f8a-422a-9374-69dfa326cd54)

This changes the folders owner 7:09 time stamp look to clear this up  
![image](https://github.com/user-attachments/assets/8511d191-94b6-44da-812d-876e0f868784)

Once changed we can now change theHive’s configuration file:<br/>

	Root > nano > /etc/thehive/application.conf/
 ![image](https://github.com/user-attachments/assets/ebb737ca-2fe5-4108-ba66-da665ef04f0a)
 ![image](https://github.com/user-attachments/assets/3f166ce7-0442-4720-be3b-37b6f62b8926)

Once you are in, Change the following:<br/>

	Scroll to hostname & Change the default into the public ip > Change the cluster-name to what you called it
![image](https://github.com/user-attachments/assets/e95f6596-61ac-4e99-8279-a1bb99b454ce)

Scroll down a bit again to change hostname to TheHives public IP > Scroll down to application.baseUrl and change “localhost” to your public ip. >save
![image](https://github.com/user-attachments/assets/e62ea7c6-a7f1-4647-81d0-003ad505e550)
![image](https://github.com/user-attachments/assets/3dad9539-8086-441a-a29f-d4dffe23a6a0)
_______________________________________________________________________________________________________________________________________
### 1d. Verifying Functionality of TheHive <br/>

	Root > systemctl start thehive> systemctl enable thehive > systemctl status thehive
![image](https://github.com/user-attachments/assets/8d44d884-9185-430e-b5de-74dc2900929b)
![image](https://github.com/user-attachments/assets/3c2518b5-a874-4e12-8acd-015cf6901f17)

Once that is done Verify that all services are running using the:<br/>

	Root> systemctl status “Cassandra” , “ElasticSearch”, TheHIVE
![image](https://github.com/user-attachments/assets/400823c8-bf4f-483b-9da8-76851e06a172)
![image](https://github.com/user-attachments/assets/95224b40-e124-4225-8832-b0346328ca28)
![image](https://github.com/user-attachments/assets/d06623b6-1eeb-40c2-9f7d-101e39a5446d)

Once verified we should be good to go! – open up a web browser and type: <br/>
“PublicIP:9000”
![image](https://github.com/user-attachments/assets/e355ce9a-70f3-4f3c-a414-6a683a8eb750)

	Login using the default:
		Username: admin@thehive.local
		password: secret

![image](https://github.com/user-attachments/assets/5be3021a-8a2d-4304-9098-0625a76d9aa1)

If you got an error like me. This indicates that elastic search might not be running
![image](https://github.com/user-attachments/assets/4ea3b274-888c-4e53-9eed-4fa5f6eb37d3)

In this case as per the video, we must create a custom jvm option file to that we must:

	Root > nano /etc/elasticsearch/jvm.options.d/jvm.options
![image](https://github.com/user-attachments/assets/44731594-b213-4ce2-93cd-27e5544c02ce)

Once you are in you can paste the following then save:<br/>

	-Dlog4j2.formatmsgNoLookups=true
	-Xms2g
	-Xmx2g

This will tell java to limit its memory usage to 2Gbs
![image](https://github.com/user-attachments/assets/2bc8e959-bb60-452a-aa6d-d1956d99036a)

Once you have saved it you should now restart elastic search

	Root > systemctl enable elasticsearch> Verify using Systemctl status elastic search
![image](https://github.com/user-attachments/assets/29c6fb37-fb55-412a-a49c-59c18a8cdfdc)

Once you have verified. Try logging in using the same default credentials!
![image](https://github.com/user-attachments/assets/a513b8cd-99cd-40c8-aaa3-79dcea52e55b)
![image](https://github.com/user-attachments/assets/c9069cf8-9b66-4291-8788-2064aa15dde9)

Once we are done we can now configure Wazuh!

____________________________________________________________________________________________
## Step 2: Configuring Wazuh <br/>
Try logging into the wazuh dashboard using the default admin credentials. If you do not have that, like I do follow the next steps.
	Verify if “wazuh-install-file.tar” is installed using the “ls" command
![image](https://github.com/user-attachments/assets/0c38c5f8-1f57-4dff-9e74-cce32ecd9222)

Extract files using the command “tar -xvf wazuh-install-files.tar” > once extracted go to the wazuh-install-files directory> ls > cat wazuh-password.txt 
![image](https://github.com/user-attachments/assets/a7c0fc36-294f-4c1a-879d-139a5785db17)
![image](https://github.com/user-attachments/assets/3a9b7b14-7e42-4caf-92b8-4ff9d8f0f76d)
![image](https://github.com/user-attachments/assets/bed70c7e-d285-402f-8e2b-b0eddf7495a8)

From here look for admin & wazuh api password (this will be used later in the project) – Save those password on a .txt file. – once you have that password log into the Wazuh dashboard!

![image](https://github.com/user-attachments/assets/977133fa-70a0-4d09-ba7a-5c4ebafc347f)
![image](https://github.com/user-attachments/assets/106092ac-7813-4265-b554-d4a2e85cf751)

Step 3: Configuring an Agent
Once we are in we can now add an Agent

	Click on Add Agent >
![image](https://github.com/user-attachments/assets/a4082955-543b-417c-b899-19f06ab4fa99)

	Select Windows > Server Address: Wazuh Public IP > Assign an Agent name: name (can be anything)
![image](https://github.com/user-attachments/assets/29111b62-f46e-4251-a711-651967d1a2e7)

	Next we can to copy the command & and open the windows 10 VM
![image](https://github.com/user-attachments/assets/9a839ff7-882e-4c87-b5fd-4a09403fc311)

	On the VM machine Open up admin Powershell > Paste the command
![image](https://github.com/user-attachments/assets/c37365bd-87dc-41cc-b059-2f4006528175)

	Start Wazuh service: net START WazuhSvc
![image](https://github.com/user-attachments/assets/6c760c9b-7e31-466e-939c-73a91009d66c)

Verify Wazuh is running via Services:
![image](https://github.com/user-attachments/assets/0494dba9-80d2-4c34-b986-30da5a2d6eda)

Now Go back into Wazuh Portal and Now you should see an active Agent
![image](https://github.com/user-attachments/assets/f2cc5b9d-adaf-4096-a2bd-45ddecf3ddaf)

Now that we have a client reporting into Wazuh. We can now query for events!
![image](https://github.com/user-attachments/assets/e5646e84-4e9b-4082-895a-b11a81376740)
