<h1>Monitoring Honeypot Attacks with Microsft Sentinel: A SIEM Home Lab Project</h1>

<h2>Description</h2>
<b>In this project, I leveraged Microsoft Azure to create and manage a cloud-based virtual machine running Windows 10. By exposing this VM to the internet, I harnessed the power of Azure Log Analytics Workspace, Microsoft Defender for Cloud, and Azure Sentinel to gather and analyze attack data, visualizing it on a map within Microsoft Sentinel. To dive deeper, I utilized PowerShell to monitor the Event Viewer on the exposed VM, specifically targeting EventID 4625 for failed logon attempts. This data was collected in a logfile, and through an API integration with IPgeolocation.io, I mapped the origins of these logon attempts using Microsoft Sentinel.This hands-on experience enriched my understanding of SIEMs, cloud services, APIs, and Microsoft Azure. I mastered the provisioning and configuration of cloud resources, gained proficiency in interpreting SIEM logs, and much more.I thoroughly enjoyed this project and hope it inspires readers to undertake similar projects or leverage the insights gained here to explore even more possibilities with Azure.<b/>
<br />

<h2>Utilities Used</h2>

- <b>Microsoft Sentinel (SIEM)</b> 
- <b>Log Analytic Workbooks</b>
- <b>Microsoft Defender for Cloud</b>
- <b>Virtual Machines</b>
- <b>Remote Desktop</b>
- <b>PowerShell</b>
- <b>API's</b>
- <b>Event Viewer</b>
- <b>Firewalls</b>

<h2>Environments Used </h2>

- <b>Microsoft Azure</b>
- <b>Windows 10</b> (21H2)

<h2>Links</h2>

- <b>Microsoft Azure Free Trial:</b> https://azure.microsoft.com/en-us/free/
- <b>IPGeolocation:</b> https://ipgeolocation.io/

<h2 align="center">Program walk-through</h2>

<p align="center">
<b>The first step in project is to create a Microsoft Azure account, which will serve as  the cloud environment for provision my resources. I'll take advantage of the $200 credit  that comes with the account to fund this project. Given that the resources I'll be using are not resource-intensive, I'll be able to save some of the credit for future endeavors. Additionally, I'll be utilizing a specific website for my IP geolocation.  </b> <br/>
</p>

![Create_Azure_Subscription](https://user-images.githubusercontent.com/108043108/225348757-c41744df-2be1-4ffc-87a6-aa258a4102ef.JPG)
![Set_Up_Completed](https://user-images.githubusercontent.com/108043108/225348950-5d9c01fa-a707-4813-a6f3-012c703c41df.JPG)
![IPGEO](https://user-images.githubusercontent.com/108043108/225456045-a2a5b61c-b0ba-49a0-9552-92bf0c8d1cf1.JPG)

<br />
<br />
<p align="center">
<b>The next step involves initiating the creation of my virtual machines. Given that provisioning a VM can be time-consuming, I'll allow it to run in the background while I proceed with other tasks.</b> <br/>
</p>

![Create_VM](https://user-images.githubusercontent.com/108043108/225350437-f6fc6e29-6821-48d5-a379-2099477589ae.JPG)

<br />
<br />
<p align="center">
<b>At this stage in the VM creation process, it's essential to create a new Resource Group to house all future resources. In Azure, a Resource Group serves as a logical container for tools, services, configurations, and more, allowing them to be managed collectively. This means they can be created or deleted simultaneously, sharing the same lifecycle. If resources are placed outside of a specific Resource Group, they will persist even if the Resource Group is deleted. Grouping resources in this way simplifies their management. I've decided to name this Resource Group "HoneyPot_Lab" and the virtual machine "HoneyPot-VM".</b>
<br/>
</p>

![New_Resource_Group](https://user-images.githubusercontent.com/108043108/225352474-3b252a72-20a6-4a56-be2a-6405d4f79cc4.JPG)
![Create_VM_2](https://user-images.githubusercontent.com/108043108/225353275-82a1ee46-586a-4ce1-9697-440c57528b3c.JPG)

<br />
<br />
<p align="center">
<b>As I continued on the same page, I selected the size for the virtual machine. Initially, I chose Standard_B1s, indicated in green in the photo. However, I later upgraded to Standard_B2s for better performance, which provided 2 CPU cores and more RAM. The first configuration was too slow; PowerShell kept crashing, and the VM was lagging significantly. After selecting the VM size, I created the admin account with a unique username and a 30-character password comprising special characters, numbers, and a mix of uppercase and lowercase letters. Given the exposed nature of the VM, a strong password was crucial to defend against brute force and dictionary attacks. Next, I configured the public inbound port rules to determine which ports would be open for connectivity to the VM. Although multiple authentication methods like SSH can be set at this step, I opted to allow only RDP, which uses port 3389.</b> <br/>
</p>

![Create_VM_3](https://user-images.githubusercontent.com/108043108/225365594-cf2fe158-d887-4bf9-aca6-d423019404a6.jpg)

<br />
<br />
<p align="center">
<b>The next step is to create a new Network Security Group (NSG). An NSG acts as a firewall, allowing you to create and enforce rules on inbound and outbound traffic to Azure resources. For this project, we don’t want to restrict traffic; we want to allow anyone and everyone to communicate with the honeypot VM. To achieve this, we'll delete the default inbound rule and create a new rule that permits all traffic into the VM. In the Destination Port Ranges box, I entered an asterisk (*) to allow any port ranges. We’ll permit any protocol as well. I set the priority at 100 because, in Azure, the lower the number, the higher the priority. This ensures that our rule takes precedence over any other potential rules. With these settings, all traffic will be allowed into our VM. It’s important to note that this configuration is only for the purpose of this project and should NEVER be used in a real production environment..</b> <br/>
</p>

![Networking_NSG_1](https://user-images.githubusercontent.com/108043108/225353983-66d6e530-783f-4db8-9720-23e575719b5f.JPG)
![Network_NSG_2](https://user-images.githubusercontent.com/108043108/225367854-f6c685bb-6e96-4981-8f20-2f3a300f1987.JPG)
![Network_NSG_3](https://user-images.githubusercontent.com/108043108/225367866-761bcc17-991d-4af1-8798-db7bca2acff4.JPG)
![Network_NSG_4JPG](https://user-images.githubusercontent.com/108043108/225367879-4aff3f39-e232-4404-aa7d-30bac028fc60.JPG)

<br />
<br />
<p align="center">
<b>As the final step, I review the configuration, create, and deploy the VM. </b> <br/>
</p>

![Create_VM_4](https://user-images.githubusercontent.com/108043108/225372110-b0dcf0b3-b991-41d5-8fbd-3817cdc56b8e.JPG)
![Deploying_VM](https://user-images.githubusercontent.com/108043108/225372118-70617d94-0808-4fcd-9a27-0413066d471d.JPG)

<br />
<br />
<p align="center">
<b>While my VM is deploying, I take the opportunity to set up the Log Analytics Workspace. These can be accessed from the Azure home dashboard or found using the search bar. When creating the Log Analytics Workspace, I ensure it's placed within my HoneyPot_Lab resource group so it can be conveniently deleted along with the group. I name this instance LAW-HoneyPot.</b> <br/>
</p>

![Log_Analytics_Workspace](https://user-images.githubusercontent.com/108043108/225373030-1633fcf3-49cf-4a6f-afa7-33337a60c57c.JPG)
![Log_Analytics_Workspace_2](https://user-images.githubusercontent.com/108043108/225373041-89acebf7-1328-4b8a-96ee-6c1a946c7df0.JPG)

<br />
<br />
<p align="center">
<b>I now go into Microsoft Defender for Cloud. I do this because I have to provision enroll in some plans to be able to collect and aggregate data for Microsoft Sentinel to be able to use later on. I also need to connect my HoneyPot-VM to Microsoft Defender so it can collect data. At this point my VM has been created so it can be connected to these services. We call these Data Connectors. In the third picture I only turn on the plans for Foundational CSPM and Servers. I'm not running any SQL servers, so it doesn't need to be turned on.</b> <br/>
</p>

![Microsoft_Defender](https://user-images.githubusercontent.com/108043108/225375649-d2e6bfc2-92d7-4193-af26-3526bf646744.JPG)
![Microsoft_Defender_2](https://user-images.githubusercontent.com/108043108/225375660-7f554560-6568-4265-a6b1-c5d6f7802774.JPG)
![Microsoft_Defender_3](https://user-images.githubusercontent.com/108043108/225375671-a99097cf-e19e-4037-98b6-a4f4c710eb94.jpg)
![Microsoft_Defender_4](https://user-images.githubusercontent.com/108043108/225375684-4825f9d7-a26d-4eb4-a477-ba3aa0b6a9b1.JPG)


<br />
<br />
<p align="center">
<b>With my VM successfully created, I can now return to Log Analytics Workspaces and connect my VM to the service.</b> <br/>
</p>

![Connect_VM_To_LAW](https://user-images.githubusercontent.com/108043108/225376430-5bb177f2-a2e9-4faa-ab8b-64bd23625407.JPG)
![Connect_VM_to_LAW_2](https://user-images.githubusercontent.com/108043108/225376446-e56ef573-901b-4d52-824d-529dad6a53d5.JPG)

<br />
<br />
<p align="center">
<b>I can now create a Microsoft Sentinel resource and link it to my VM.</b> <br/>
</p>

![Azure_Sentinel](https://user-images.githubusercontent.com/108043108/225377514-f5462e65-4b40-4823-a613-7088cfeff63a.JPG)
![Azure_Sentinel_2](https://user-images.githubusercontent.com/108043108/225377521-f4d379cf-58d7-4aca-aee0-1a80cb9aceac.JPG)

<br />
<br />
<p align="center">
<b>With everything set up in the Azure dashboard, I can now configure my VM. The first step is to obtain my VM's public IP address to establish a Remote Desktop (RDP) connection. I navigate to the Virtual Machines tab in Azure, select the HoneyPot VM, and note the highlighted public IP address.</b> <br/>
</p>

![Getting_VM_Public_IP](https://user-images.githubusercontent.com/108043108/225379998-5bcad4c5-c878-4249-8384-34e1581dc35d.JPG)

<br />
<br />
<p align="center">
<b>From this point, I can log into my VM via Remote Desktop. I open Remote Desktop on my local PC, enter the public IP address and necessary credentials, and establish the connection. Once connected, I proceed to configure the HoneyPot.</b> <br/>
</p>

![Connect_With_RDP](https://user-images.githubusercontent.com/108043108/225381141-3514bcc6-3417-4281-ad67-1b4af20edfd5.JPG)
![Logging_Into_VM_Via_RDP](https://user-images.githubusercontent.com/108043108/225381164-d6591c73-4cf0-4fee-85c9-22d56abfc8ab.JPG)
![Logging_Into_VM_via_RDP_2](https://user-images.githubusercontent.com/108043108/225662102-2a73273a-c0f7-4424-833f-2d981f5859e0.JPG)

<br />
<br />
<p align="center">
<b>After successfully logging into the VM via RDP, I navigate to Event Viewer. Event Viewer records all activities on a Windows system, assigning each action an EventID for easier navigation. For this project, we are specifically concerned with EventID 4625, which denotes Failed Logon Attempts. These logs can be found in the Security tab. In the images below, I launch another instance of Remote Desktop and attempt to log into the VM with an incorrect password. This generates a failed logon attempt, which is then logged and recorded in Event Viewer.</b> <br/>
</p>

![Event_Viewer](https://user-images.githubusercontent.com/108043108/225381724-2603d72a-5334-479d-b555-c1d0baaeb875.JPG)
![Event_Viewer_2](https://user-images.githubusercontent.com/108043108/225381737-0bc2d129-b045-4099-a5fe-d34b4d43f673.JPG)


<br />
<br />
<p align="center">
<b>The next step is to ping the VM from my local computer to determine if a direct connection can be made and to check its discoverability. The VM is currently not discoverable because Windows Defender Firewall is active, blocking ICMP Echo Requests. I confirmed this by pinging the VM's public IP address, 74.235.173.155, and observing multiple "Request timed out" messages. The firewall is dropping the ICMP request packets.</b> <br/>
</p>

![Ping_VM](https://user-images.githubusercontent.com/108043108/225383503-2a599790-511e-47ec-b858-4055f42ead0c.JPG)

<br />
<br />
<p align="center">
<b>To ensure that everyone on the internet can discover my VM, I need to disable the Windows Firewall. I start by searching for wf.msc in the Windows search bar. Once inside the Windows Firewall settings, I proceed to disable all firewall rules. Then, I open Command Prompt (CMD) on my local computer and ping the VM again. This time, it successfully receives replies from the VM because the firewall is no longer blocking ICMP requests. </b> <br/>
</p>

![Turn_Off_Firewall](https://user-images.githubusercontent.com/108043108/225385096-6cdeaa2a-3411-46fa-bad1-0afcee031860.JPG)
![Turn_Off_Firewall_2](https://user-images.githubusercontent.com/108043108/225385112-d4ada032-3cdc-4650-b60a-0a373cddfd17.JPG)

<br />
<br />
<p align="center">
<b>With my VM now exposed to the internet, I can begin setting up the PowerShell script, which is the core of this project. This script will parse Event Viewer logs, specifically targeting EventID 4625, which indicates failed logon attempts. It will then send the IP addresses from these failed attempts to the website IPgeolocation.iovia an API. This approach is used because Event Viewer logs do not include geographical location information. By leveraging a dedicated service, I can obtain this data more efficiently than building the functionality from scratch. The PowerShell script will receive the geographical data and save it as a string in a logfile named failed_rdp.log. Later in the project, this logfile will be used to map the attacks live in Microsoft Sentinel.</b> <br/>
</p>

![Create_PS_Script](https://user-images.githubusercontent.com/108043108/225409638-0f9735a1-eca0-446f-afb7-74ef0d427ebf.JPG)


<br />
<br />
<p align="center">
<b>After opening PowerShell, I paste my pre-written script and save it to the desktop as Log_Exporter. Next, I create a profile on IPgeolocation to obtain my API key. This key is then integrated into the PowerShell script. While you'll get 1,000 free API calls, they can be consumed quickly. Therefore, I recommend re-enabling the Windows Firewall until you've finished setting up your Log Analytics Workbooks Custom Fields later in the lab. Once those configurations are complete, you can disable the Firewall again.</b> <br/>
</p>

![Create_PS_Script_2](https://user-images.githubusercontent.com/108043108/225410802-01a83b34-e79a-4516-8bdb-70c01baa76d7.JPG)
![Create_PS_Script_3](https://user-images.githubusercontent.com/108043108/225410821-7078f7c1-1928-49c4-8e03-a123ef43cb3f.JPG)

<br />
<br />
<p align="center">
<b>I ran my Log_Exporter script and customized it to display outputs in pink and black for better readability. The API_KEY shown in the photos has been changed after completing the project. The first photo demonstrates that the script is functioning correctly, displaying the first failed login attempt I performed earlier. The second photo shows the data being saved in string format into the failed_rdp logfile. I included sample data in this file to enhance the accuracy of training the AI in Log Analytics Workbooks and Microsoft Sentinel, as more data leads to greater precision.</b> <br/>
</p>

![Run_PS_Script](https://user-images.githubusercontent.com/108043108/225412357-ee390c08-c131-4658-b6d0-b2c6f0485850.JPG)
![Training_Sentinel](https://user-images.githubusercontent.com/108043108/225412372-30c65d96-6678-4ac1-a3a9-d0f3d8331342.JPG)

<br />
<br />
<p align="center">
<b>To test if the PowerShell script was working, I intentionally failed another logon attempt. Surprisingly, someone quickly found my VM and began attempting to brute force it. This individual was located in Tunisia and discovered the VM so rapidly that it was slightly frustrating. Although I considered blocking the IP or re-enabling the firewall until I completed my setup, the data being generated was ideal for training the AI in Azure, so I decided to let it continue. In hindsight, this was a mistake. The free API from IPgeolocation.ioallowed for only 1,000 calls, and the attacker from Tunisia quickly exhausted that limit, disrupting my project. Consequently, I had to spend $15 for an additional 150,000 API calls to salvage my project. </b> <br/>
</p>

![Showing_PS_Script_Someone_Already_Found_VM](https://user-images.githubusercontent.com/108043108/225413356-343d9ae2-240d-4841-b41f-62cff77e51ab.JPG)

https://user-images.githubusercontent.com/108043108/225419861-a5c9bce1-3f6d-42e9-9bd4-dcd7ca816a33.mp4

<br />
<br />
<p align="center">
<b>With my PowerShell script confirmed to be working, I proceed to Log Analytics Workbooks to create a custom log for integrating my failed_rdp log into Log Analytics. In Log Analytics, I navigate to my VM and create a Legacy Custom Log. It prompts for a sample log, which resides in the VM. Since I can't download the logfile directly to my local computer, I open the logfile within the VM, copy its contents, then transfer this data to my local machine by pasting it into Notepad and saving it to my desktop. From there, I import the file into Log Analytics Workbooks. This sample data will be used to train Log Analytics.</b> <br/>
</p>

![Custom_Logs](https://user-images.githubusercontent.com/108043108/225421446-5a0c5c4f-62a0-4a07-bb6a-b36edd43b7fd.JPG)
![Custom_Log_2](https://user-images.githubusercontent.com/108043108/225421932-a4d520b3-5f9a-4851-badb-e07744735144.JPG)
![Custom_Log_3](https://user-images.githubusercontent.com/108043108/225421947-dcaadf65-e212-41ae-8e83-322a310df6f7.JPG)
![Custom_Log_4](https://user-images.githubusercontent.com/108043108/225421956-8dd57a76-b0b0-44a0-8f10-675ba6b1dfa9.JPG)
![Custom_Log_5](https://user-images.githubusercontent.com/108043108/225421995-664b1d05-5ba7-48a3-9e90-a1c5eedeed36.JPG)

<br />
<br />
<p align="center">
<b>Next, it asks for the collection path, which is the location of the logfile within the VM. This path is essential for Log Analytics to access and collect log data. The path to the file is C:\ProgramData\failed_rdp.log. If the path is incorrect, Log Analytics won't be able to retrieve the log information. Following that, we need to name our custom log. I chose the name FAILED_RDP_WITH_GEO, and the .CL (Custom Log) suffix will be automatically appended. When querying the database later, this will essentially serve as the table name. Finally, we create the custom log. </b> <br/>
</p>

![Custom_Log_6](https://user-images.githubusercontent.com/108043108/225422804-46df9880-9734-420c-84a9-e18ec3a68b57.JPG)
![Custom_Log_7](https://user-images.githubusercontent.com/108043108/225423680-580feffc-72e3-4eaf-9fd0-57d0376976b3.JPG)
![Custom_Log_8](https://user-images.githubusercontent.com/108043108/225424273-b392d383-4025-4206-b406-0fd79058d263.JPG)

<br />
<br />
<p align="center">
<b>While the provisioning process is instant, it will take some time for the data to sync from the VM to Log Analytics. In the meantime, I decided to query the Event Viewer, which should have already synced. As shown in picture 1, it displays all the logs correctly. After a short while, I queried the newly created FAILED_RDP_WITH_GEO custom log, and it successfully showed information, indicating that the VM and Log Analytics are synced and exchanging data.</b><br/>
</p>

![Security_Event_4625](https://user-images.githubusercontent.com/108043108/225425000-75d4b1ae-fa60-48a4-af30-8fc5ab52cb8f.JPG)
![Failed_RDP_LOGS](https://user-images.githubusercontent.com/108043108/225425211-162958fe-13cb-455a-99bf-2b24897a5a29.JPG)

<br />
<br />
<p align="center">
<b>Now, I need to extract the fields used in my log, which will allow me to utilize those fields later in Microsoft Sentinel. I start by right-clicking a failed RDP login log that contains all the raw data from my PowerShell script and highlighting the data I want. I then name the field for the data.After this extraction, the Log Analytics AI reviews all other sample data and actual logs generated to identify and pull the correct data. At this stage, I need to correct any errors the AI makes by highlighting the correct data points. To do this, I scroll through the list (highlighted in blue in picture 4), right-click any incorrect entries, and re-highlight the correct information.The custom fields I create at this stage include: latitude, longitude, destinationhost, username, sourcehost, state, country, label, and timestamp.</b><br/>
</p>

![Extract_Fields](https://user-images.githubusercontent.com/108043108/225436517-0a82f48e-82c8-45cf-b351-634ee8ba41c7.JPG)
![extract_data_2](https://user-images.githubusercontent.com/108043108/225436525-d57e194f-8a55-4e9a-915b-9d8a413a83e4.JPG)
![extract_data_3](https://user-images.githubusercontent.com/108043108/225436533-e68158a6-3cd5-4f84-a6b2-1329a1339016.JPG)
![extract_data_4](https://user-images.githubusercontent.com/108043108/225436540-4ec75b85-8076-493e-9fb7-8c5bc24cba4b.JPG)


<br />
<br />
<p align="center">
<b>I waited a while to see if the fields would populate properly. All of them did, except for sourcehost.CL, which I couldn't figure out why. I deleted and re-extracted that field multiple times, but it still wouldn't populate. Since an unpopulated field couldn't be used in my Sentinel live map, I ultimately decided to delete it and exclude that data point.</b>  <br/>
</p>

![Sourcehost_wouldnt_update](https://user-images.githubusercontent.com/108043108/225451005-edabe896-94b1-4d11-8853-42e4a4ce8e83.JPG)
![sourcehost_wouldnt_update_2](https://user-images.githubusercontent.com/108043108/225451011-80b1dbad-2e90-4407-bfa5-5c516b66a991.JPG)

<br />
<br />
<p align="center">
<b>The next step was to set up a geomap that pinpoints and maps out where the attacks or login attempts were originating. I did this by navigating to Microsoft Sentinel. In the first picture, you can see that the SIEM has been properly collecting and categorizing data. Although I didn't set any alerts for this project, it's certainly a possibility for future endeavors. The picture shows nearly 10,000 events and 6,900 security events, sourced from Event Viewer in the VM, with 2,300 failed RDP attempts.</b>  <br/>
</p>

![Setting_Geomap](https://user-images.githubusercontent.com/108043108/225451297-e0c57af2-5333-4b6f-8fff-79b11b816e77.JPG)

<br />
<br />
<p align="center">
<bMoving on, to create the map I want, I'll need to create a new workbook in Sentinel. After clicking into Workbooks, I'll delete the default graphs and widgets. Once those are removed, I'll start creating a new workbook.</b>  <br/>
</p>

![setting_geomap_2](https://user-images.githubusercontent.com/108043108/225452457-2c805584-bee1-4c4b-adb3-aa73726c0d0f.JPG)
![setting_geomap_3](https://user-images.githubusercontent.com/108043108/225452473-99fd3ef3-04ff-49e6-aecf-94793b7e60f2.JPG)
![setting_geomap_4](https://user-images.githubusercontent.com/108043108/225452571-ef8a7a55-a541-44ba-9269-24a32d56d14c.JPG)

<br />
<br />
<p align="center">
<b>To create the map, I need to add a query. This query will pull the data and fields from Log Analytics, essentially specifying the dataset I want Microsoft Sentinel to use. In the query, I ensure to specifically exclude (!=) data points that include "Samplehost," as these are not real attacks and I don't want them to populate on the map.</b>  <br/>
</p>

![setting_geomap_5](https://user-images.githubusercontent.com/108043108/225453225-24b50358-ac8c-4897-9e44-8d21f4075239.JPG)

<br />
<br />
<p align="center">
<b>After I queried the data points I want, I now have to choose how I want to express/visualize them. In this case I want them visualized as a map! I choose the map setting and then configure the map to plot the attacks by latitude/longitude. I could do it by country but some of the attacks coming through was not including the country. I changed the metric settings to make the bubbles bigger using the event counts. The more events the bigger the bubble. I apply the settings and my map is done!</b>  <br/>
</p>

![setting_geomap_6](https://user-images.githubusercontent.com/108043108/225453707-62496864-0c33-4d3d-988c-3260b07e9c9b.JPG)
![setting_geomap_7](https://user-images.githubusercontent.com/108043108/225454149-b35a2412-574f-474a-acca-3b541148208a.JPG)
![setting_geomap_8](https://user-images.githubusercontent.com/108043108/225454159-593b96e9-e6dc-4cbc-b6fa-bcb955495bfd.JPG)


<br />
<br />
<p align="center">
<b>After a few hours and right before I decided to stop the project, you can see that there was a total of 10,529 attacks or failed login attempts. 20 were in the USA, which was me testing the PowerShell script, 9 from Cambodia, and a whopping 10.5k from Tunisia. They were using automated brute-forcing software to try thousands of different password combinations and usernames. There were even more attempts, but my PowerShell script had to be stopped and started multiple times. This is why it's important to use strong passwords and uncommon usernames! The second picture is the number of API calls I had that day. All of them are not shown because I had to upgrade the number of calls I could make. </b>  <br/>
</p>

![Stopped_Because_of_API_Calls_Limit_At_150k](https://user-images.githubusercontent.com/108043108/225454550-20b2f3d6-5593-44cf-8bbc-290fb941da8c.JPG)
![API_requests](https://user-images.githubusercontent.com/108043108/225455054-0576753d-b3d0-40dd-aa4b-70246b1616e8.JPG)

<br />
<br />
<p align="center">
<b>After I was done, I had to delete the resource group I created for this project. If I left it alone, it would eat up my $200 credit I would need for future projects. The reason I put everything under one resource group is for this exact reason, easier deletion of the resources I created. Now nothing would be left behind.</b>  <br/>
</p>

![Deleting_Resources](https://user-images.githubusercontent.com/108043108/225455713-94e78229-b9ec-4f2e-ae17-24ed2636f7a1.jpg)
![deleting_resources_2](https://user-images.githubusercontent.com/108043108/225455718-29980ca4-9976-4abf-955f-573f6d55fdd9.JPG)

<br />
<br />
<h2 align="center">Decided To Run The Lab Again.</h2>
</p>

<br />
<br />
<p align="center">
<b>I decided to repeat the lab to give attackers more time to target the exposed VM, ensuring a broader representation of locations on the world map. This decision proved successful, as a more diverse group of threat actors attacked my VM this time, resulting in a richer version of the world map.</b>  <br/>
</p>

![worldmap2](https://user-images.githubusercontent.com/108043108/226672121-d80091a0-6f7b-4f1d-b5bd-d9416823c6bf.JPG)

<br />
<br />
<p align="center">
<b>While running this lab a second time, I noticed an issue where my PowerShell script would periodically stop updating. Although it was still running, no new entries were generated for 5 to 15 minutes. This was puzzling, given that I knew multiple countries were attempting to brute-force their way into the VM, which should have resulted in a steady stream of new entries. I had set the refresh rate in this PowerShell script to 300 milliseconds, down from 1 second in the earlier lab, to ensure timely updates. To investigate, I decided to compare the number of security events with Event ID = 4625 against the number of Failed_RDP_Custom_Logs generated. I accomplished this by writing queries in Log Analytics Workbook.</b>  <br/>
</p>

![Security_Event_ID](https://user-images.githubusercontent.com/108043108/226673169-7b2f64a3-7ab1-4918-98e8-b680a553aabf.jpg)
<p align="center"><i>After writing my query, it is clear that there were a total of 9,282 EventID 4625 Security Events. This indicates that threat actors attempted to log into my VM 9,282 times.</i></p>  <br/>

![Failed_RDP_Log](https://user-images.githubusercontent.com/108043108/226673194-787f3452-309d-4d0b-a375-c653fb16c4b1.JPG)
<p align="center">
<i>After writing this query we can see it returned a total of 3892 records, which means that my Custom Log only captured 3892 login attempts. Quite a bit away from 9282.</i>
</p>

<br />
<br />
<p align="center">
<b>Okay, there is a discrepancy here. Both numbers of records should match, or at least be very close to each other. Next, I decided to examine my API usage for today, which shows 3,541 API requests. This figure is much closer to my Custom Log FAILED_RDP_CUSTOM_LOG. Therefore, I now know that it isn't my VM failing to send the data to my custom log.</b>  <br/>
</p>

![API](https://user-images.githubusercontent.com/108043108/226676332-d906603b-59c1-4ad0-94c0-fb665bb26a73.JPG)

<br />
<br />
<p align="center">
<b>Lastly, I decided to check Microsoft Sentinel to see what the dashboard was displaying. My PowerShell script was still running in the background, so the numbers aren't an exact match since the screenshots were taken a few minutes apart. However, it shows a number (4,100) that is much closer to the FAILED_RDP_CUSTOM_LOG query./b>  <br/>
</p>

![Sentinel](https://user-images.githubusercontent.com/108043108/226676717-a08eb2c2-6ae1-46f7-8d78-7359b2d610bf.JPG)

<br />
<br />
<h2 align="center">Conclusions</h2>

<br />
<br />
<p align="center">
<b>What I suspect is happening is that the Windows Event Viewer is working correctly, logging all login attempts and showing the actual number of occurrences. The weak link in this process appears to be the PowerShell script. The reason for the script pausing for extended periods is likely due to it being overwhelmed. I set the refresh rate to 300 milliseconds, but the number of login attempts from multiple attackers exceeded 1 attempt per 300 milliseconds. Consequently, the PowerShell script became a bottleneck. If it could record each attempt as it happened, it would align with the Event Viewer’s numbers. However, since it could only log one attempt per 300 milliseconds, some login attempts were either lost or not processed in real-time, leading to the discrepancy observed. In the future, I might consider lowering the refresh rate even further to allow for more login attempts to be captured accurately.</b>  <br/>
</p>


<br />
<br />

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
