<h1>SIEM - Microsoft Sentinel Lab</h1>

<!--
 ### [YouTube Demonstration](https://youtu.be/7eJexJVCqJo)
-->

<h2>Description</h2>
In this project, I set up a SIEM (Azure Sentinel) and connect it to a live virtual machine (VM) on Azure running a Windows environment. By lowering defenses, I set this VM up as a honeypot. Interestingly, attacks can be observed from around the world attempting to RDP Brute Force the VM. With a custom PowerShell script, I'm able to look up the attackers' geolocation information using Free IP Geolocation API and plot these attackers' locations on the Azure Sentinel Map.
<br />


<h2>Languages and Utilities Used</h2>

- <b>PowerShell</b> 
- <b>Azure Sentinel</b>
- <b>Kusto Query Language</b>

<h2>Environments Used </h2>

- <b>Windows 10 Pro N</b> (22H2)

<h2>Program walk-through:</h2>

<p align="center">
Sign-up for a free Azure account: <br/>
<img src="https://i.imgur.com/LERWDY4.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
Select Virtual machines at the top:  <br/>
<img src="https://i.imgur.com/wH906kr.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
Select Create -> Azure virtual machine: <br/>
<img src="https://i.imgur.com/wa1WQEt.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
Configure the following selections as such:  <br/>
  I chose (US) East US since West would not work with selected configurations.  <br/>
  Security type: Standard: <br/>
<img src="https://i.imgur.com/x5OVCkB.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
Create a custom username (avoid admin/administrator), password, confirm licensing, and select Next: <br/>
<img src="https://i.imgur.com/039d2Lu.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
Select next on Disks page and under Networking -> NIC security group select Advanced then Create New: <br/>
<img src="https://i.imgur.com/b7HKyKc.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
Remove existing inbound rule and add a new rule with the following configurations (name can be anything)  <br/>
  ****The purpose of this rule is allow traffic in to practice on the SIEM. DO NOT DO THIS ON A LIFE ENVIRONMENT!  <br/>
  Select OK, Review+Create, Create:  <br/>
<img src="https://i.imgur.com/uylcdf0.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
While the VM is being created, use the search at the top to locate "Log Analytics workspaces":  <br/>
<img src="https://i.imgur.com/0eSICeJ.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
Select "Create log analytics workspace", use your existing Resource group, and give the Instance a name  <br/>
  Review and Create: <br/>
<img src="https://i.imgur.com/vRL9lfF.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
Next we want to enable the ability to gather logs from the VM into the log analystics workspace. <br/>
  Navigate to "Microsoft Defender for Cloud" with the search bar. <br/>
  Locate and select "Environment Settings" from the left navigation pane. <br/>
  Select the log analytics workspace just created (in my case, "loganalytics-honeypot"): <br/>
<img src="https://i.imgur.com/mvYM0bp.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
Turn "Servers" ON for Microsoft Defender. Leave "SQL servers on machines" OFF since they're irrelevant to the lab. <br/>
  Hit Save (if it fails, try again until the Azure accepts the change): <br/>
<img src="https://i.imgur.com/NAWZpS6.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
Select "Data collection" on the left, Select "All Events" and Save: <br/>
<img src="https://i.imgur.com/NAWZpS6.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
Navigate to "Log Analytics Workspace", select your created workspace, and select "Virtual machines": <br/>
<img src="https://i.imgur.com/FzJb61J.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
Select your VM: <br/>
<img src="https://i.imgur.com/kCmq0WI.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/> <br/>
Connect: <br/>
<img src="https://i.imgur.com/SmVUpTp.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
Navigate to "Microsoft Sentinel" and select "Create Microsoft Sentinel": <br/>
<img src="https://i.imgur.com/UhpDdlt.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
Select the log analytics workspace created earlier and select "Add": <br/>
<img src="https://i.imgur.com/Qyxmb9a.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
Navigate to "Virtual machines", select your VM, and copy the Public IP address: <br/>
<img src="https://i.imgur.com/RqzJQF9.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
Search for Remote Desktop Connection on your computer, and input the public IP from previous step:<br/>
<img src="https://i.imgur.com/TfsYFFe.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
After inputing IP, select "More choices" and "Use a different account". <br/>
 Input the account name and password you created on Azure. <br/>
 Allow VM to load: <br/>
<img src="https://i.imgur.com/3F7euXH.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
***ON HOST COMPUTER, NOT VM*** <br/>
 Open Command Prompt (CMD) and enter: ping xxx.xxx.xxx.xxx -t <br/>
 Put the IP Address of your VM where xxx.xxx.xxx.xxx is<br/>
 You'll notice pings are timing out: <br/>
<img src="https://i.imgur.com/22nJXp3.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
***ON VM, NOT HOST COMPUTER*** <br/>
 Select start, type "wf.msc" and Select "Windows Defender Firewall Properties":
<img src="https://i.imgur.com/KTugpCm.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
***ON VM, NOT HOST COMPUTER*** <br/>
On Domain, Private, and Public Profiles, turn Firewall state to Off. Hit Apply and OK <br/>
 Again, this is being performed ON THE VM to monitor attacks. Do not conduct this on your HOST COMPUTER!!! <br/>
<img src="https://i.imgur.com/KTugpCm.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
 <img src="https://i.imgur.com/4G1U29l.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
***ON HOST COMPUTER, NOT VM*** <br/>
Verify that pings are not reaching the VM: <br/>
<img src="https://i.imgur.com/TRnFqy8.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
***ON VM, NOT HOST COMPUTER*** <br/>
 Select start, type "Event Viewer" and Navigate to "Windows Logs" -> "Security" <br/>
 This will show us the event logs for login successes or failures <br/>
 In particular, you may see Event ID == 4625 which is a logon failure <br/>
 Our goal is to collect these logon failures from RDP attackers and plot them on the SIEM: <br/>
<img src="https://i.imgur.com/Dy0Tef8.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
***ON HOST COMPUTER, NOT VM*** <br/>
Go to [ipgeolocation](https://ipgeolocation.io), signup, and grab your API Key number. <br/>
 ***NOTE: Free API Key is only good for 1000 API Requests. Not much considering a single attacker could flood you with thousands of attempts into your VM. <br/>
 You can purchase a subscription to see more data, but it is not required to learn the necessary skills for this lab. <br/>
<img src="https://i.imgur.com/ZUo6dWx.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
 ***ON VM, NOT HOST COMPUTER*** <br/>
 This step allows us to extract the geolocation of the attackers (converting IP Addresses to latitude and longitude). <br/>
 Select start, type "Windows PowerShell ISE" to open an instance. Hit "New Script" at the top left <br/>
 Copy the PowerShell script from the file on this repository and paste it into the VM's PowerShell instance <br/>
 Replace the API Key (d4600...a457) from the PowerShell script with YOUR API Key from ipgeolocation <br/>
 Save the file to the Desktop as "Log_Exporter" on Desktop (ensure it has .ps1 type underneath): <br/>
<img src="https://i.imgur.com/3k3lghi.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
 ***ON VM, NOT HOST COMPUTER*** <br/>
 Push play (red circle) to run the script and leave it running. <br/>
 You will begin to see attacks from varying parts of the world attempting to break in. <br/>
 Here we're receiving attacks from Argentina. They're trying a variety of different usernames to gain access. <br/>
<img src="https://i.imgur.com/Xl2bVUX.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
***ON HOST COMPUTER, NOT VM*** <br/>
At this point, our PowerShell script is collecting a log (ON THE VM) of the geolocation of our attackers. <br/>
 What we can do now is go back into our Azure Portal and create a custom log to pull the data from the log on our VM. <br/>
 Search for Log Analytics workspace -> select your existing workspace. <br/>
 Select Tables -> Create -> New custom log (MMA-based) <br/>
<img src="https://i.imgur.com/vxRJg92.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
 ***ON VM, NOT HOST COMPUTER*** <br/>
Select Start -> type "Run" -> Search for C:\ProgramData (this folder is hidden by default) <br/>
 Open the txt file "failed_rdp", copy all of it's contents. <br/>
 ***ON HOST COMPUTER, NOT VM*** <br/>
Open a notepad on your host computer, and paste the contents from your VM's failed_rdp log file. <br/>
 Save it on your Desktop as "failed_rdp.log". <br/>
 Back in Azure within "Create a custom log", it is asking for a sample log. Select the file you saved on your host computer's Desktop "failed_rdp.log" <br/>
 This will be used to train Log Analytics on what to look for on the actual file (within our VM). <br/>
 Select Next <br/>
<img src="https://i.imgur.com/g5dlrST.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
 ***ON HOST COMPUTER, NOT VM*** <br/>
In Azure, hit next until you reach Collection paths <br/>
 Type = "Windows" and Path = "C:\ProgramData\failed_rdp.log" <br/>
 Select Next <br/>
<img src="https://i.imgur.com/1K37OGB.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
 ***ON HOST COMPUTER, NOT VM*** <br/>
Under details, give the log a custom name <br/>
 I used "FAILED_RDP_WITH_GEO" <br/>
 Hit Next and Create <br/>
<img src="https://i.imgur.com/1K37OGB.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
 ***ON HOST COMPUTER, NOT VM*** <br/>
It will take some time for our custom log to populate from the VM to Azure Log Analytics <br/>
 We can verify failed Remote Desktop Protocol (RDP) events within Azure logs <br/>
 Type this query "SecurityEvent | where EventID == 4625" and Hit Run <br/>
<img src="https://i.imgur.com/0r3PyYf.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
 ***ON HOST COMPUTER, NOT VM*** <br/>
From the last step, we were able to see the Windows log events. <br/>
 Now, we'll query our log. Input the full KQL query to extract RawData separating columns for latitude, longitude, etc. <br/>
 The full query is in this repository under "AzureKQLQuery" <br/>
 Verify that the output matches what's on your VM's log before proceeding to the next step. <br/>
<img src="https://i.imgur.com/3qxtaKr.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
 ***ON HOST COMPUTER, NOT VM*** <br/>
Lastly, we will use the same query from above to create a map within Azure Sentinel to plot the attacks. <br/>
 Navigate to Azure -> Microsoft Sentinel -> Select current Sentinel project -> Worksbooks -> Add Workbook <br/>
<img src="https://i.imgur.com/5TqDZdc.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
 ***ON HOST COMPUTER, NOT VM*** <br/>
Select Edit on the top left, and then the 3 dots on the right to remove the 2 existing modules. <br/>
<img src="https://i.imgur.com/QbwXMvf.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
 ***ON HOST COMPUTER, NOT VM*** <br/>
Select + Add and "Add query" <br/>
<img src="https://i.imgur.com/mzbedOm.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
 ***ON HOST COMPUTER, NOT VM*** <br/>
Paste the same KQL query from earlier. <br/>
 Under Visualization, select "Map" and hit "Run query" <br/>
 (You may also change the "Size" to "Full" if you'd like a better view) <br/>
<img src="https://i.imgur.com/AnQVcbb.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
 ***ON HOST COMPUTER, NOT VM*** <br/>
Apply the following settings to adjust the map to see country and amount of log entries (RDP attempts): <br/>
 (If your results look off, try using "country" field rather than latitude and longitude) <br/>
<img src="https://i.imgur.com/TSWbSb2.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<img src="https://i.imgur.com/dI8T6Np.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />
 ***ON HOST COMPUTER, NOT VM*** <br/>
Name and save your map and view where the attacks are coming from <br/>
 The API only grants 1000 free calls a day, so on this day, I received attacks from Argentina. <br/>
 The 2 logs were from my local machine logging in <br/>
<img src="https://i.imgur.com/tsC5xF0.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
<br />
<br />

<h2>Thanks for viewing my home lab. Hope you get to try it out sometime!
Aloha, Jeremy</h2>

 
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
