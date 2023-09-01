<h1>Setting up Microsoft Sentinel (Azure SIEM) <br />to display global attacks on honeypot</h1>

<h2>Description</h2>
In this project, I created an exposed virtual machine with Windows Firewall turned off to entice global attacks. I configured a log repository in Azure and used a custom PowerShell script, then setup Microsoft Sentinel (Azure's SIEM) to read the data and plot the attacks on a map. This project helped me get a good grasp on configuring and navigating a SIEM, as well as its other capabilities, albeit there are still tons to learn about. The diagram is as follows:
<img src="https://imgur.com/5nrZcwb.png" height="80%" width="80%" alt="Network Diagram"/>
<br />
- <b>Create and configure a VM using Microsoft Azure's service with very low security settings.</b> <br />
- <b>Use custom PowerShell to forward extracted data to third party API to obtain geolocation data..</b> <br />
- <b>Setup and configure Log Analytics Workspace in Azure to ingest custom logs containing geographic information.</b> <br />
- <b>Configure Microsoft Sentinel (Azure SIEM) workbook to display global attack data on world map using custom fields in Log Analytics Workspace. </b> <br />

<h2>Languages and Utilities Used</h2>

- <b>Microsoft Sentinel (Azure SIEM)</b>
- <b>Log Analytics Workspace</b>
- <b>PowerShell</b>
- <b>ipgeolocation.io API</b>

<h2>Environments Used </h2>

- <b>Windows 10 Pro(21H2)</b>
- <b>Microsoft Azure</b>

<h2>Program walk-through:</h2>

<p align="center">
Create a Windows 10 Virtual Machine on Microsoft Azure and configure its settings to allow public RDP (3389): <br/>
<img src="https://i.imgur.com/uoO9327.png" height="40%" width="40%"/>
<img src="https://i.imgur.com/9U148FZ.png" height="40%" width="40%"/>
<br />
<br />
Configure the Network Security Group's Inbound Rule to allow all traffic with a low priority to entice attackers:<br/>
<img src="https://i.imgur.com/nvWhyzB.png" height="80%" width="80%"/>
<br />
<br />
Once configured, add a Log Analytics Workspace service from Azure and set it in the same resource group.<br/>
<img src="https://i.imgur.com/et7jPiI.png" height="80%" width="80%"/>
<br />
<br />
Next is to navigate to Microsoft Defender for Cloud and go to Environmental Settings. There,<br />  configure Defender's Server plan to "ON" and SQL Servers to "OFF" and Data Collection to "All Events". <br/>
<img src="https://i.imgur.com/Hy6fLc7.png" height="40%" width="40%"/>
<img src="https://i.imgur.com/ZWPIGLa.png" height="40%" width="40%"/>
<br />
<br />
Go back to Log Analytics Workspace and navigate to Virtual Machines to connect the VM we created earlier.<br/>
<img src="https://i.imgur.com/Ed4hsW9.png" height="80%" width="80%"/>
<br />
<br />
Next is to add Microsoft Sentinel (Azure SIEM) and also select the VM that we created. <br/>
<img src="https://i.imgur.com/SJXQxWU.png" height="80%" width="80%"/>
<br />
<br />
Return to the Virtual Machine we created and check the IP Address found on the <br />Overview page. This is what we'll use to connect to Remote Desktop Protocol (3389) <br/>
<img src="https://i.imgur.com/O8GT1Ow.png" height="80%" width="80%"/>
<br />
<br />
Using our own PC, connect to the given IP Address via Remote Desktop Connection <br />and use the credentials we configured when we created our VM. <br/>
<img src="https://i.imgur.com/4U4Jl2n.png" height="80%" width="80%"/>
<br />
<br />
Continue and setup Windows, and proceed to turn off its Domain, Private, and Public profile's <br /> Firewall state. This would further entice attackers and also allow ICMP using our own PC.<br/>
<img src="https://i.imgur.com/GjEqowj.png" height="40%" width="40%"/>
<img src="https://i.imgur.com/DUNhinb.png" height="40%" width="40%"/>
<br />
<br />
Open Event Viewer and navigate to Windows Logs > Security, these will be the logs <br />that we'll be extracting, specifically Logon Failures, to plot RDP attacks.<br/>
<img src="https://i.imgur.com/hvC3lZ1.png" height="80%" width="80%"/>
<br />  
<br />
Using ipgeolocation's API, we'll use the attackers' IP Addresses to create a custom log that <br />contains these information, which we will then send to Log Analytics Workspace and Microsoft Sentinel.<br/>
<img src="https://i.imgur.com/gmUydvD.png" height="80%" width="80%"/>
<br />  
<br />
Use custom PowerShell Script to generate a log file containing all the data and information from <br />ipgeolocation's API. Get your API KEY from the website and insert inside the script.<br/>
<i>This will note all the failed logons on this VM's RDP.</i><br/>
<img src="https://i.imgur.com/NIFNLYp.png" height="80%" width="80%"/>
<br />  
<br />
Next is to create a custom log inside of our Log Analytics Workspace. This will allow us to read <br/>the custom logs we generated to forward to Microsoft Sentinel (Azure SIEM).<br/>
<img src="https://i.imgur.com/2PZG8rn.png" height="40%" width="40%"/>
<img src="https://i.imgur.com/2oQCP5z.png" height="40%" width="40%"/>
<br />  
<br />
Now add a custom query that extracts all the relevant data from the log and summarizes it for Microsoft Sentinel (Azure SIEM).<br/>
<img src="https://i.imgur.com/D6twPSN.png" height="80%" width="80%"/>
<br />  
<br />
Navigate to Microsoft Sentinel (Azure SIEM) and go to Workbooks. Here, we'll add the custom query we used earlier and feed it to the SIEM.<br/>
We'll then visualize these information to a world map, showing the attackers trying to access our RDP.<br/>
<img src="https://i.imgur.com/c9n9LpG.png" height="80%" width="80%"/>
<br />  
<br />
Whenever a failed logon to our RDP occurs, the Log Analytics Workspace custom query will log it, which then will be read by Microsoft Sentinel. <br />All the data will be accumulated and visualized via the world map.
We'll leave our virtual machine overnight to allow the attack attempts to increase and see a more meaningful visualization on our world map.<br/>
<img src="https://i.imgur.com/c4MvGES.png" height="80%" width="80%"/>
<br />  
<br />
After approximately 24 hours and hundreds of failed logon attempts later, this is what our world map looks like inside Microsoft Sentinel (Azure SIEM).<br/>
<img src="https://i.imgur.com/lastna.png" height="80%" width="80%"/>
<br />  
<br />
<b>This project will be worked on further in the future to create more complex configurations and scenarios.</b>
</p>
