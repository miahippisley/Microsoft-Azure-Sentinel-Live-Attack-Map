<h1>SIEM tutorial: Live Cyber Attacks Map Using Azure Sentinel </h1>

<h2>Description</h2>
This project uses Microsoft Azure and a virtual machine in the cloud running Windows 10. The VM computer system is set-up to be intentionally compromised, to make it discoverable and enticing to hackers. A log analytics workspace is configured in Azure to ingest custom logs containing information such as latitude, longitude, country and state/province. The log data is collected, queried using Azure Sentinel (the SIEM we use to visualise the data), and displayed in real-time on a heat map showing the physical location and magnitude of the attempted attacks.
<br />
<br />
<img width="468" alt="image" src="https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/680fdfc5-8fe6-4b6b-992d-18209f61dae2">
<br />
<h2>Tools and requirements:</h2>

- <b>Microsoft Azure</b> 
- <b>Services within Azure: Log Analytics Workspace and Sentinel (Microsoft's SIEM)</b>
- <b>Kusto Query Language (KQL)</b> 
- <b>Remote Desktop Protocol (RDP)</b>
- <b>[PowerShell Script written by Josh Makadour](https://github.com/joshmadakor1/Sentinel-Lab/blob/main/Custom_Security_Log_Exporter.ps1)</b> 
- <b>[3rd Party API](https://ipgeolocation.io/)</b>

<h2>Overview</h2>

<h2>Step 1: Create a free Azure subscription</h2>

[Microsoft Azure](https://azure.microsoft.com/en-gb/free/search/?ef_id=_k_CjwKCAjwjaWoBhAmEiwAXz8DBSoVqr5MvGTl_hPm1ICTlXr-qRvUEvbXY2GDLQTxZLlndplAer3LwRoCdTAQAvD_BwE_k_&OCID=AIDcmm3bvqzxp1_SEM__k_CjwKCAjwjaWoBhAmEiwAXz8DBSoVqr5MvGTl_hPm1ICTlXr-qRvUEvbXY2GDLQTxZLlndplAer3LwRoCdTAQAvD_BwE_k_&gad=1&gclid=CjwKCAjwjaWoBhAmEiwAXz8DBSoVqr5MvGTl_hPm1ICTlXr-qRvUEvbXY2GDLQTxZLlndplAer3LwRoCdTAQAvD_BwE) offer a free £200 credit for 30 days

<h2>Step 2: Create a Virtual Machine</h2>
1. Search for "Virtual Machines" in the search bar of the "Quickstart Center" page
<br />
2. Select Create > Azure Virtual Machine. Create a new resource group and name it something - mine is called "exposed-lab"
<br />
<br />
3. <b>Apply the following settings:</b>
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
4. Click Next to Disc and leave it as the default settings, then continue to Networking
<br />
5. Select NIC network security group > Advanced
<br />
6. Select Configure network security group > Create new
<br />
7. Remove the default rule
<br />
8. Select Add an inbound rule
<br />
<br />
9. <b>Apply the following settings:</b>
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
1. Return to the Search bar and enter 'Microsoft Defender for Cloud'
<br />
<br />
<img width="468" alt="image" src="https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/2ead69f1-77a4-4b96-bd75-ebab0ab359a5">
<br />
<br />
2. Uncheck SQL servers and press save
<br />
<br />
3. Select Data collection > All Events > Save
<br />
<br />
<img width="468" alt="image" src="https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/a36dacd0-676d-4393-a532-36ebfbc3a922">
<br />
<br />
<h2>Step 5: Connect Log Analytics to VM</h2>
1. Search for log analytics workspace and select your workspace
<br />
2. Select Virtual Machines > (virtual machine name e.g. exposed-lab) > Connect
<br />
<br />
<h2>Step 6: Setup Azure Sentinel</h2>
1. Search for Microsoft Sentinel, this is our SIEM we will use to visualise the data.
<br />
2. Select Create > (your log analytics workspace name e.g. law-exposedlab)
<br />
<br />
<h2>Step 7: Log into VM with Remote Desktop</h2>
1. Search for Virtual Machines > (virtual machine name)
  <br />
2. Copy the Public IP address
<br />
3. Open Windows Remote Desktop
  <br />
4. Paste the IP Address vopied from the virtual machine. If you are using a Mac, select 'Add PC' and then paste the IP address.
  <br />
5. After creating your remote desktop, double-click and create a username and password.
<br />
<br />
<img width="269" alt="image" src="https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/f24464c2-fed5-4645-a36e-dbe1d3ae5837">

<img width="468" alt="image" src="https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/8c02535f-ca0b-462a-8c94-9f1bccf75391">
<br />
<br />
<h2>Step 8: Turn off Windows Firewall in VM</h2>
1. Inside the VM, click Start and search for 'wf.msc'
<br />
2. Select Windows Defender Firewall Properties
<br />
<br />
<img width="468" alt="image" src="https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/9eb7db4f-0507-41c1-b751-6f27e24919bc">
<br />
<br />
<img width="468" alt="image" src="https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/6d406450-309f-4518-9a3a-0489ae4f10a0">
<br />
<br />
3. Disable domain, public and private firewalls
<br />
<br />
<img width="306" alt="image" src="https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/39dba72f-622e-4514-9071-b8e785ccd939">
<br />
<br />
<h2>Step 9: Running a PowerShell Script</h2>

1. Open the following [PowerShell Script](https://github.com/joshmadakor1/Sentinel-Lab/blob/main/Custom_Security_Log_Exporter.ps1) made by Josh Makadour 
<br />
2. Open PowerShell ISE > New > Copy/Paste the PowerShell script above > Save to desktop as 'logexporter'
<br />
<br />
<img width="468" alt="image" src="https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/7b2805f3-1d56-41b4-8f37-1878248f3db6">
<br />
<br />
3. Sign up to [Free IP Geolocation API and Accurate IP Lookup Database](https://ipgeolocation.io/![image](https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/120312df-9d3d-4afa-9999-3abc28ccac71)
<br />
<br />
4. You will be given your own API key. Replace the API key in the PowerShell script with this, to allow you to get the geographical data.
<br />
<br />
5. Run the script. This will perpetually collect log data and create a new log file. 
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
1. Search Log Analytics Workspace > (your workspace name) > Tables > Create > New Custom Log (MMA Based)
<br />
<br />
You will be asked to select a log file, but our log file will be on the virtual machine, rather than the host computer. 
<br />
<br />
2. Open the log file in the VM, select all and copy.
<br />
3. Open a notepad on your host computer, paste the log file and save the text file. 
<br />
<br />
<img width="468" alt="image" src="https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/fb1b66b7-d419-4bbe-9099-1809732bfa87">
<br />
<img width="468" alt="image" src="https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/df4f4b1c-c58b-48a1-8ef8-5113a23d057d">
<br />
<br />
4. Return to Microsoft Azure and select your new file as the log file. 
<br />
5. Select Collection paths and input the location of the file in the <b>virtual machine</b> 
`C:\ProgramData\failed_rdp.log`
<br />
6. Select Details, name your custom log 'FAILED_RDP_WITH_GEO' and save. 
<br />
<br />
<img width="468" alt="image" src="https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/25e34840-b787-4e99-aef4-a03a6ddc9220">
<br />
<img width="468" alt="image" src="https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/7533b12c-ef63-4b67-ae2c-73a4500f8906">
<br />
<br />
It will take about 30 minutes for it to sync with the virtual machine. 
<br />
<br />
<h2>Step 10: Extract fields from raw custom log data</h2>
The log data collects multiple pieces of geographical information (e.g. longitude, latitude, city, country), so we need to parse the data into the multiple properties. 
<br />
1. Go to Log Analytics Workspaces > (your workspace name) > Logs
<br />
2. Run the following query:

```
FAILED_RDP_WITH_GEO_CL
| parse RawData * "latitude:" Latitude ",longitude:" Longitude ",destinationhost:" DestinationHost ",username:" Username ",sourcehost:" Sourcehost ",state:" State ", country:" Country ",label:" Label ",timestamp:" Timestamp
| where DestinationHost != "samplehost"
| where Sourcehost != ""
| summarize event_count=count() by Sourcehost, Latitude, Longitude, Country, Label, DestinationHost
![image](https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/5b987d34-f83e-4cbf-b258-2d2f31dae5d6)
```

<img width="468" alt="image" src="https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/c12ea4da-d7ac-484a-95d3-213415a5934a">
<br />
<br />
<h2>Step 11: Map the Data in Sentinel</h2>
1. Search for Sentinel in Microsoft Azure and click on your log analytics workspace.
<br />
<br />
<img width="468" alt="image" src="https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/e70c02c8-2e78-463c-b74b-02d2859c5e77">
<br />
<br />
2. Select Workbooks > Add Workbook > Edit
<br />
<br />
3. Remove the two default widgets that come with the workbook.
<br />
<br />
<img width="468" alt="image" src="https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/2d4f90eb-8ee5-4740-a367-718586608927">
<br />
<br />
4. Select Add > Add query
<br />
5. Copy/Paste the following query into the query window and select run:

```
FAILED_RDP_WITH_GEO_CL 
| extend username = extract(@"username:([^,]+)", 1, RawData),
         timestamp = extract(@"timestamp:([^,]+)", 1, RawData),
         latitude = extract(@"latitude:([^,]+)", 1, RawData),
         longitude = extract(@"longitude:([^,]+)", 1, RawData),
         sourcehost = extract(@"sourcehost:([^,]+)", 1, RawData),
         state = extract(@"state:([^,]+)", 1, RawData),
         label = extract(@"label:([^,]+)", 1, RawData),
         destination = extract(@"destinationhost:([^,]+)", 1, RawData),
         country = extract(@"country:([^,]+)", 1, RawData)
| where destination != "samplehost"
| where sourcehost != ""
| summarize event_count=count() by timestamp, label, country, state, sourcehost, username, destination, longitude, latitude
![image](https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/6331376c-baee-4615-9997-9492ef69a444)

```

6. Select Visualisation > Map 

<img width="468" alt="image" src="https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/1cbde4de-3db3-4e3a-b4dd-e43cb620720b">
<br />
<br />
7. In Map Settings, apply the following: 
<br />
<br />
<b>Layout Settings</b> 
<br />
<br />
Location info using: Latitude/Longitude
<br />
Latitude: latitude_CF
<br />
Longitude: longitude_CF
<br />
Size by: event_count
<br />
<br />
<b>Color Settings</b> 
<br />
<br />
Coloring Type: Heatmap
<br />
Color by: event_count
<br />
Aggregation for color: Sum of values
<br />
Color palette: Green to Red
<br />
<br />
<b>Metric Settings</b> 
<br />
<br />
Metric Label: label_CF
<br />
Metric Value: event_count
<br />
<br />
Select Apply > Save > Close. Save as 'Failed RDP World Map', in the same region and resource group. 
<img width="468" alt="image" src="https://github.com/miahippisley/Microsoft-Azure-Sentinel-Live-Attack-Map/assets/127256439/091ad4d4-5a41-4379-a017-a2e970d3208d">
<br />
<br />
As you refresh the map, additonal incoming failed RDP attacks will come in. 

<h2>Step 12: IMPORTANT: Deprovision</h2>
Search Resource groups > (your research group) > Delete resource group
<br />
Check 'Apply force delete for Virtual machine and Virtual machine scale sets' and delete. 
<br />
<br />
