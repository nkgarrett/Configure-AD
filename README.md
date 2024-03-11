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
<img width="500" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/36532058-cf06-4f9e-97be-f67756e132dc"><br><br>
Next you'll install Active Directory.<br><br>
- - - - 
First, switch back to DC-1. Then you'll go to Server Manager -> Add roles and features -> Active Directory Domain Services<br><br>

(NOTE: Make sure to only select Active Directory Domain Services as there are multiple Active Directory available)<br><br>
<img width="500" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/6dca0038-7695-4079-bc87-62e61dd46fd2"><br><br>
<img width="750" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/b4fb92e7-3dca-4d39-ac33-170afd5deb64"><br><br>
Add features and select next and install.<br><br>
- - - -

Next you'll promote the server as a Domain Controller. In the upper right corner you'll see a flag with an action sign next to it in the Server Manager notifications section. You'll select promote this server as a domain controller.

<img width="500" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/3fff1813-ee92-4114-bbb4-e0a3ac43c72c"><br><br>
Select Add new forest then set up a new forest as mydomain.com. Conversely it can be named anything, just remember what you choose.<br><br>
<img width="500" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/56399e71-6415-4736-b3dc-1e6e04cc32df"><br><br>
Choose a password on the next page and just click through 'Next' and install. Once the installation is complete the host will restart.<br><br>
<img width="500" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/e0765bf7-50ad-47cd-bdd0-454415eefd01"><br><br>
Return to DC-1 in Azure and refesh the VM for the public ip address so you can log back into the remote desktop. The ip address should be the same but it might change upon refresh. DC-1 is now a domain controller, so you'll have to long in with the context of the fully qualifed domain name (FQDN) as username. So: yourdomainname.com\whatever username thats a member of this domain, which you created when you initally set up the VM.

i.e.: mydomain.com\labuser<br><br>
<img width="500" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/9ef33876-2d71-42eb-a504-c1b7258c89f7"><br><br>
- - - -

Now you'll create an Admin and Normal User Account in Active Directory.<br><br>

From the search bar on DC-1, you'll locate Active Directory Users and Computers and you'll right click to create an Organizational Unit (OU) named '_EMPLOYEES' (the underscore and caps are not necessary, but they do help to eaisly differentiate the OU's you create from the other containers) and another OU named '_ADMINS' then simply right click on the domain to refresh.<br><br>
<img width="500" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/b76e3b37-8c33-49c2-bbf0-80a0b3dccd71"><br><br>
Next you'll go to the _ADMINS OU, open it up and right click inside for New -> Users and add new user. Fill in the name and create a password.<br><br>
<img width="500" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/1274265f-b1b5-44e6-816d-e975334b4042"><br><br>
Next you'll add your new user to the Domains Admins Security Group to make them a domain admin. Right click your user to open properties -> Member of -> Add and type 'domain admins' in the box and select Check Names and it should underline, then Apply your changes and select ok. <br><br>
<img width="500" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/822bb784-3acb-4353-a708-350e8318ea48"><br><br>
Next you'll log out of DC-1 and reopen Remote Desktop and log back in as your newly created admin user.<br><br>

i.e.: mydomain.com\jane_admin<br>
Next you'll join Client-1 to your domain.<br><br>
- - - -

From the Azure Portal you'll set Client-1’s DNS settings to the DC’s Private IP address.<br>
Select Client-1 -> Network Settings -><br><br>
<img width="500" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/cd36ab66-344c-4f39-92e3-1a18a7c49980"><br><br>
NIC -><br><br>
<img width="500" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/39029c94-d68c-46b2-b984-11e46cb63ca9"><br><br>
DNS servers -><br><br>
<img width="500" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/d39620d2-5b77-487c-aef4-24968c12e83d"><br><br>
Custom -><br><br>
<img width="500" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/5ad73b36-586c-4fe1-bffb-3de49f0fcdea"><br><br>
Enter DC-1's private ip and save.<br><br>
<img width="500" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/70dd3344-bc79-4e11-b5a5-ba4984046859"><br><br>
Now from the Azure portal, restart Client-1.<br><br>
<img width="500" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/170e0b76-8a02-4ad2-a00b-59983000072d"><br><br>
Next you'll login to Client-1 (Remote Desktop) as the original local admin (original username only, same domain name) and join it to the domain.<br><br>

Right click start -> System -><br><br>
<img width="500" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/f0210c2d-ef3c-4022-bb06-d7f4d102ade8"><br><br>
Rename this pc advanced -><br><br>
<img width="500" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/0d61a73b-17b4-481a-a677-b0230f30aa89"><br><br>
Change -><br><br>
<img width="500" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/cfdb69e0-c54a-48cb-8d65-cdfbe8623b06"><br><br>
Domain -><br><br>
<img width="410" alt="image" src="https://github.com/nkgarrett/Configure-AD/assets/156832893/0fc6baf9-0215-4b84-a4fa-9c0dd3e435c5"><br><br>
Enter the domain name you created in the box (i.e.: mydomain.com) -><br><br>

Enter the credentials of the domain admin account -> yourdomainname.com(admin user) -> enter admin users password. Welcome to (domainaddress).com should populate somewhere behind the open windows.<br><br>
















