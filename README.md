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

<h2>Step 1: Create an [azure subscription] (https://azure.microsoft.com/en-gb/get-started/azure-portal)</h2>

<h2>Step 2: Create a Virtual Machine</h2>
Search for 'Virtual Machines' in the search bar of the "Quickstart Center" page.
Select Create > Azure Virtual Machine. Create a new resource group and name it something - mine is called "exposed-lab". 

![Step2](<img width="469" alt="Step2" src="https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/73f399aa-82df-4287-aa1d-89c3766699bb">)


