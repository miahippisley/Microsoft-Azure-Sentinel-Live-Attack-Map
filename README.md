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

<h2>Step 1: Create a free Azure subscription</h2>
Microsoft Azure offer free £200 credit for 30 days

<h2>Step 2: Create a Virtual Machine</h2>
- Search for "Virtual Machines" in the search bar of the "Quickstart Center" page.
<br />
- Select Create > Azure Virtual Machine. Create a new resource group and name it something - mine is called "exposed-lab". 
<br />
<br />
<b>Apply the following settings:</b>
<br />
<br />
- Region: (Europe) UK South
<br />
- Security type: Standard
<br />
- Image: Windows 10 Pro, version 22H2 - x64 Gen2
<br />
- VM architecture: x64
<br />
- Size: Standard_D2s_v3 - 2 vcpus, 8 GiB memory
<br />
- Create a username and password and keep note of them
<br />
<br />
<img width="468" alt="image" src="https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/dd98f605-29a8-43e7-a5f3-00b479664b7f">
<br />
<br />
Click Next to Disc and leave it as the default settings, then continue to Networking.
<br />
- Select NIC network security group > Advanced
<br />
- Select Configure network security group > Create new
<br />
- Remove the default rule
<br />
- Select Add an inbound rule
<br />
<br />
<b>Apply the following settings:</b>
<br />
- Source: Any 
<br />
- Source port ranges: *
<br />
- Destination: Any 
<br />
- Service: Custom
<br />
- Destination port ranges: *
<br />
- Protocol: Any
<br />
- Priority: 100
<br />
- Name: DANGER_ANY_IN
<br />
<br />
This will allow all traffic from the internet into the lab; we want the virtual machine to be enticing to hackers. 
<br />
<br />
<h2>Step 3: Create a Log Analytics Workspace</h2>
Return to the Search bar and visit 'Log Analytics Workspaces' and create a workspace. Add it to the same resoruce group and region as your VM and name your workspace. You can name it anything, but mine is called "law-exposedlab". 
<br />
<br />
<img width="468" alt="image" src="https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/6f316c72-c9d2-4eeb-b33d-41a321069861">
<br />
<br />
We are making a custom log that contains geographic information so we can discover where the attacks are coming from.
<br />
<br />
<h2>Step 4: Configure Microsoft Defender for Cloud</h2>
Return to the Search bar and enter 'Microsoft Defender for Cloud'. 
<br />
<br />
<img width="468" alt="image" src="https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/2ead69f1-77a4-4b96-bd75-ebab0ab359a5">
<br />
<br />
Uncheck SQL servers and press save. 
<br />
<br />
Select Data collection > All Events > Save
<br />
<br />
<img width="468" alt="image" src="https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/a36dacd0-676d-4393-a532-36ebfbc3a922">
<br />
<br />
<h2>Step 5: Connect Log Analytics to VM</h2>
- Search for log analytics workspace and select your workspace
<br />
- Select Virtual Machines > (virtual machine name e.g. exposed-lab) > Connect
<br />
<br />
<h2>Step 6: Setup Azure Sentinel</h2>
Search for Microsoft Sentinel, this is our SIEM we will use to visualise the data.
<br />
<br />
- Select Create > (your log analytics workspace name e.g. law-exposedlab)
<br />
<br />
<h2>Step 7: Log into VM with Remote Desktop</h2>
- Search for Virtual Machines > (virtual machine name)
  <br />
- Copy the Public IP address
<br />
<br />
- Open Windows Remote Desktop
  <br />
- Paste the IP Address vopied from the virtual machine. If you are using a Mac, select 'Add PC' and then paste the IP address.
  <br />
- After creating your remote desktop, double-click and create a username and password.
<br />
<br />
<img width="269" alt="image" src="https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/f24464c2-fed5-4645-a36e-dbe1d3ae5837">

<img width="468" alt="image" src="https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/8c02535f-ca0b-462a-8c94-9f1bccf75391">
<br />
<br />
<h2>Step 8: Turn off Windows Firewall in VM</h2>
<br />
<br />
- Inside the VM, click Start and search for 'wf.msc'
<br />
- Select Windows Defender Firewall Properties
<br />
<br />
<img width="468" alt="image" src="https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/9eb7db4f-0507-41c1-b751-6f27e24919bc">
<br />
<br />
<img width="468" alt="image" src="https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/6d406450-309f-4518-9a3a-0489ae4f10a0">
<br />
<br />
- Disable domain, public and private firewalls
<br />
<br />
<img width="306" alt="image" src="https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/39dba72f-622e-4514-9071-b8e785ccd939">
<br />
<br />
<h2>Step 9: Running a PowerShell Script</h2>

Open the following [PowerShell Script](https://github.com/joshmadakor1/Sentinel-Lab/blob/main/Custom_Security_Log_Exporter.ps1) made by Josh Makadour 
<br />
<br />
Open PowerShell ISE > New > Copy/Paste the PowerShell script above > Save to desktop as 'logexporter'
<br />
<br />
<img width="468" alt="image" src="https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/7b2805f3-1d56-41b4-8f37-1878248f3db6">
<br />
<br />
Sign up to [Free IP Geolocation API and Accurate IP Lookup Database](https://ipgeolocation.io/![image](https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/120312df-9d3d-4afa-9999-3abc28ccac71)
<br />
<br />
You will be given your own API key. Replace the API key in the PowerShell script with this, to allow you to get the geographical data.
<br />
<br />
Run the script. This will perpetually collect log data and create a new log file. 
<br />
<br />
As per the PowerShell script, log file will be named 'failed_rdp.log' and its location is
`C:\ProgramData\failed_rdp.log`.
<br />
<br />
<h2>Step 9: Create a custlom log in LAW to bring in our custom log</h2>
We will now create a custom log inside our log analytics workspace that will allow us to bring the custom log containing the geodata into our log analytics workspace.
<br />
<br />
- Search Log Analytics Workspace > (your workspace name) > Tables > Create > New Custom Log (MMA Based)
<br />
<br />
You will be asked to select a log file, but our log file will be on the virtual machine, rather than the host computer. 
<br />
<br />
- Open the log file in the VM, select all and copy.
<br />
- Open a notepad on your host computer, paste the log file and save the text file. 
<br />
<br />
<img width="468" alt="image" src="https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/fb1b66b7-d419-4bbe-9099-1809732bfa87">
<img width="468" alt="image" src="https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/df4f4b1c-c58b-48a1-8ef8-5113a23d057d">
<br />
<br />
- Return to Microsoft Azure and select your new file as the log file. 
<br />
- Select Collection paths and input the location of the file in the <b>virtual machine</b> 
`C:\ProgramData\failed_rdp.log`
<br />
- Name your custom log 'FAILED_RDP_WITH_GEO' and save. 













