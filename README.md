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
First you'll go to Network Settings -><br>
<img width="500" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/7369b210-8923-4fae-8535-7ceaa0d1033b"><br><br>
NIC -><br><br>
<img width="500" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/fef51dd1-3c48-4218-8d63-2b6de40b0488"><br><br>
IP configuration -><br><br>
<img width="500" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/b9c85807-31ba-4e09-afcb-d56f6da433cf"><br><br>
Then on the Edit IP configuration page you'll change the allocation from Dynamic to Static and select save.<br><br>
<img width="500" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/9875e834-f848-47ba-84fc-d7bab4cb60d1"><br><br>


