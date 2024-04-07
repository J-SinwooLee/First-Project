# Microsoft Sentinel SIEM Wrold Map Project

<hr>
<h3>Overview</h3>
In this project, I'll illustrate the process of configuring Azure services and demonstrate their application in setting up a honeypot. By creating this environment, we can observe and analyze attacker activity as they attempt to access our virtual machine. At the end of the project, we will create a map using Microsft Sentinel to overview where the attackers were coming from as well as how many attacks were attempted. Additionally, we'll utilize Microsoft Sentinel to generate a map, providing insights into the origins of the attacks and the frequency of attempted breaches. This project was inspired by Josh Madakor's SIEM guide, and all credit goes to him.
<hr>

<h4>Cloud Tools and Evnironment</h3>
AZURE VM, VNET, Log Ananlytic Workspace, Microsoft Sentinel, Windonw ver: win10-22h2-pro-g2
<h4>Languauge and Unitlites</h3>
PowerShell

<h2>Project walk-through</h2>
<hr>

<h4>Step 1: How to set up Azure honeypotVM</h4>
<b2>
  
  1. Click "Virtual Machine" as the screen below
  2. If you cannot find the icon, then you can search it on the search bar
</b2>
<img src="1.png">

<b2>
On the next screen you will click "Create".
</b2>
<img src="2.png">

<b2>
Create a new resource group by clicking "Create new".

For this instance, I will create a resource group name: FirstProject.
<img src="3.png">
</b2>

<b2>
Now we are going to name our VM as HoneypotVM. Choose West US 3 for region. For the image, you will choose either Windows 10 or 11.
<img src="4.png">
</b2>

<b2>
"IMPORTANT"

Username and password will be used for RDP to remotely login in to VM. If the password is too easy then attackers can possiblly brute force themselves in. Be adivsed to create password with this in mind.
Once, everything is setup. Click "Next" and get to "Networking" tab.

<img src="5.png">

<b2>
We are going to name our virtual network as HoneypotVM-net. Then click on "Advance" next to Configure Network security group then "Create new".

<img src="6.png">

Click the trash can icone to delete the default setting

<img src="7.png">
Then click "Add an inbound rule"

<img src="8.png">
</b2>

<b2>
Change "Destination port range" to * for Any
"Action" to "Allow"
Priority: 100
We'll name this "ANY_ALLOWED_DANGER", but feel free to choose any other name you prefer.
When everything is set correctly click "Add" then "Ok".
At last click "Review + Create"

<img src="9.png">
</br2>

<h4>Step 2: How to set up Log in Azure</h4>
<br2>
Now VM is going to be created and it will take a moment.

Meanwhile we should work on creating Log.

1. Click Log Analytic Workspace
2. If you do not see the icon, you can search it on the search bar

Then click "Create"
<img src="10.png">

<br2>
Resource group is going to be the same as the VM and we are going to name this log as LAW-HoneypotVM. It will have the same region as the VM as well. Then click "Review + Create"
<img src="11.png">
</br2>

<h4>Step 3: How to set up Microsoft Defender for Cloud</h4>
<br2>
Again we can either search for the service or click on the icon.
<img src="12.png">
Then click on "Environment settings" > Subscription name > LAW-HoneypotVM
<img src="13.png">
<img src="14.png">

On Defender plans page
<br>
Foundational CSPM - ON
<br>
Servers - ON
<br>
SQL servers on machine - OFF 
<br>
then click "save"
<img src="15.png">

Then move to "Data collection" on the left side. Click "All Events" then click "save"
<img src="16.png">

<h4>Step 4: Connecting services</h4>
<br2>
Now that we've set up all the necessary services, the next step is to establish connections between them to enable communication.
</br2>

<br2>
Let's go backto Log Analaytic Workspace then click "Virtual Machine" on the list then click on the VM that you created. Then click on "Connect".
<img src="17.png">

<h4>Step 5: Microsoft Setinel (SIEM) setup</h4>
<br>
You can search or click on Microsofot Sentiel icon.
<img src="18.png">
Then click "Create". Your Log Analytic Workspace will come up which in this case will be LAW-HoneypotVM.
Click the Log then "click Add".

<h4>Step 6: Remote into VM and VM configuration</h4>
<br>
Go back to "Virtual Machine" on Azure and click on the VM that you have created then you will find "Public IP address" on overview. 
<br>
Copy the address then login to VM via RDP on your PC. 

Then you will use the credential that you created the VM in Step 1.
<img src="19.png">

"IMPORTANT"
This action has to be done on your VM not on your PC. Doing it on PC can cause serious problem.

Once you have successfully logged in.
Search for "wf.msc", then click "Windows Defender Firewall Properties".
<img src="20.png">
Turn off the firewall on "Domain Profile", "Private Profile", and "Public Profile"
By turning them off VM will reponse do the ICMP echo request which will enable people to discover this VM.
We do not want to do this to our PC.
<img src="21.png">

<h4>Step 7: Using PowerShell to send logs to SIEM</h4>
<br>
In VM search for "PowerShellISE" and start it.
Copy the code from then save it as "Log_Exporter"
<br>
https://github.com/joshmadakor1/Sentinel-Lab/blob/main/Custom_Security_Log_Exporter.ps1
<br>
Please note that you'll need your own API KEY in the script, which you can obtain by signing up at ipgeolocation.io. The daily limit is 1,000 queries, which resets every 24 hours.
<br>
You can make an account on https://ipgeolocation.io/
<br>
Once you've added your API KEY to the script, run the code. You'll soon notice a purple script appearing, indicating that "attackers" have discovered the VM and are attempting to log in unsuccessfully. The script will provide valuable information about the attackers.
<img src="22.png">




















