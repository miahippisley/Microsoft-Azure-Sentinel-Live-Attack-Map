<h1>SIEM tutorial: Cyber Attacks Map Using Azure Sentinel </h1>

<h2>Description</h2>
This project uses Microsoft Azure and a virtual machine in the cloud running Windows 10. The VM computer system is set-up to be intentionally compromised, to make it discoverable and enticing to hackers. A log analytics workspace is configured in Azure to ingest custom logs containing information such as latitude, longitude, country and state/province. The log data is collected, queried using Azure Sentinel (the SIEM we use to visualise the data), and displayed in real-time on a heat map showing the physical location and magnitude of the attempted attacks.
<br />

<h2>Tools and requirements:</h2>

- <b>Microsoft Azure</b> 
- <b>Services within Azure: Log Analytics Workspace and Sentinel (Microsoft's SIEM)</b>
- <b>Kusto Query Language (KQL)</b> 
- <b>Remote Desktop Protocol (RDP)</b>
- <b>Powershell script written by Josh Makadour:</b> 
- <b>[3rd Party API](https://ipgeolocation.io/)</b>

<h2>Overview</h2>

h2>Step 1: Create an [azure subscription] (https://azure.microsoft.com/en-gb/get-started/azure-portal)</h2>

<p align="center">
Launch the utility: <br/>
<img src="https://i.imgur.com/62TgaWL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Select the disk:  <br/>
<img src="https://i.imgur.com/tcTyMUE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Enter the number of passes: <br/>
<img src="https://i.imgur.com/nCIbXbg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Confirm your selection:  <br/>
<img src="https://i.imgur.com/cdFHBiU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Wait for process to complete (may take some time):  <br/>
<img src="https://i.imgur.com/JL945Ga.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Sanitization complete:  <br/>
<img src="https://i.imgur.com/K71yaM2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Observe the wiped disk:  <br/>
<img src="https://i.imgur.com/AeZkvFQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>

