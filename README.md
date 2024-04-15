<h1>SIEM - Azure Sentinel Lab</h1>

<!--
 ### [YouTube Demonstration](https://youtu.be/7eJexJVCqJo)
-->

<h2>Description</h2>
In this project, I set up a SIEM (Azure Sentinel) and connect it to a live virtual machine (VM) on Azure running a Windows environment. By lowering defenses, I set this VM up as a honeypot. Interestingly, attacks can be observed from around the world attempting to RDP Brute Force the VM. With a custom PowerShell script, I'm able to look up the attackers Geolocation information using Free IP Geolocation API and plot these attackers' locations on the Azure Sentinel Map.
<br />


<h2>Languages and Utilities Used</h2>

- <b>PowerShell</b> 
- <b>Azure Sentinel</b>

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
  Hit Save: <br/>
<img src="https://i.imgur.com/NAWZpS6.png" height="80%" width="80%" alt="Azure Sentinel SIEM Steps"/>
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
