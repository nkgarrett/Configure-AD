<h1>Active Directory Deployed in the Cloud (Azure)</h1>

This tutorial outlines the implementation of Active Directory within Azure Virtual Machines.<br>

<h2>Environments and Technologies Used</h2>

* Microsoft Azure (Virtual Machines/Compute)
* Remote Desktop<br>
* Active Directory Domain Services<br>
* PowerShell<br>

<h2>Operating Systems Used</h2>

* Windows Server 2022<br>
* Windows 10 (22H2)<br>
<h2>High-Level Deployment and Configuration Steps</h2>

* Setup Resources in Azure<br>
* Set Domain Controller’s NIC Private IP address to static<br>
* Ensure Connectivity between the client and Domain Controller<br>
* Enable ICMPv4 in on the local windows Firewall and check connectivity<br>
* Install Active Directory<br>
* Create an Admin and Normal User Account in Active Directory<br>
* Join Client-1 to your domain<br>
* Setup Remote Desktop for non-administrative users on Client-1<br>
* Create additional users and log into client-1 with one of the users<br>
* Deployment and Configuration Steps
<h2>Deployment and Configuration Steps</h2>
Create the Domain Controller VM (Windows Server 2022) named 'DC-1'. Take note of the Resource Group ( which can be created on the VM creation page or from the Resource Group creation page ) and Virtual Network (Vnet) that get created here. Select a Region to place both your VMs and you'll want to select a size that has atleast 2 vcpus; anything less will cause the OS on your VM to run slowly. Once everything is filled out, review and create your VM.<br><br>
<img width="500" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/260be658-094f-4dab-bfb9-f9311631ff13"><br><br>

- - - -

First you'll go to Network Settings -><br><br>
<img width="500" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/7369b210-8923-4fae-8535-7ceaa0d1033b"><br><br>
NIC -><br><br>
<img width="500" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/fef51dd1-3c48-4218-8d63-2b6de40b0488"><br><br>
IP configuration -><br><br>
<img width="500" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/b9c85807-31ba-4e09-afcb-d56f6da433cf"><br><br>
Then on the Edit IP configuration page you'll change the allocation from Dynamic to Static and select save.<br><br>
<img width="500" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/9875e834-f848-47ba-84fc-d7bab4cb60d1"><br><br>

Next you'll create your second VM named 'Client-1'. Make sure to use the same Resource group as DC-1, check 'licensing' then select Next: Disks and then Next: Networking to reach the networking page.<br>

Ensure that both VMs are in the same Vnet. (NOTE: if the Vnet from DC-1 isnt showing in the drop dwon menu, then allow more time for the Vnet to fully populate before creating your Client-1 VM)<br>
<img width="500" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/c1978be4-d245-4b76-8606-a5db6c52d259"><br>
<img width="500" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/8f1fe94e-277e-44fa-bf67-2199322fc3aa"><br><br>
Once you've verified the Vnet, select Review + create and if your validation passes you can create your VM.<br><br>
- - - -

Next you'll ensure connectivity between the client and Domain Controller.
Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t (DC-1 private ip address)<br><br>
<img width="500" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/38c0f730-9b48-49ca-be87-e0d109fce4ef"><br><br>
<img width="471" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/812d272c-5ce8-4061-a9fa-d7d62dbb20b4"><br><br>

The ping will time out so you'll need to start another instance of remote desktop and login to the Domain Controller to enable ICMPv4 in on the local windows Firewall.<br><br>

From the search bar in DC-1 you'll type wf.msc to open Microsoft Common Console Document -> Inbound Rules<br><br>
<img width="500" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/b7339af2-e14f-429b-879e-717bb24765c2"><br><br>
Sort by protocol -> ICMPv4 -><br><br>
<img width="500" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/8de70d08-3d5c-42d8-a9b2-8d1907130aa3"><br><br>
Right click to enable Core Network Diagnosis rules.<br><br>

Return to Client-1 and the ping should now be responding<br><br>
<img width="500" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/36532058-cf06-4f9e-97be-f67756e132dc">



