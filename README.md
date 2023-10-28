<h1>Active Directory - Home Lab</h1>

<h2>Description</h2>
In the Active Directory Home Lab project, I created a functional Active Directory environment using Oracle VirtualBox. This involved setting up a domain controller, configuring IP addressing, and installing Active Directory services. I also automated user creation with PowerShell and created a client machine connected to the domain. This hands-on experience has deepened my understanding of Active Directory and Windows networking, providing valuable skills for network administration and system management.
<br />


<h2>Languages and Utilities Used</h2>

- <b>PowerShell</b> 
- <b>Oracle Virtual Box</b>

<h2>Environments Used </h2>

- <b>Windows 10</b> (21H2)
- <b>Server 2019</b>

<h2>Program walk-through:</h2>

<p align="center">
The network diagram illustrates the Active Directory Home Lab's structure. It features a Domain Controller (DC), a Client Machine, and two network connections: Internal (for internal communication) and External (providing internet access and routing for private network clients). This visual aids in understanding the project's network configuration. <br/>
<br />
<img src="https://imgur.com/UGgLac7.png" height="80%" width="80%" alt="Network Diagram"/>
<br />
<br />
<p align="center">The first phase involved downloading essential components: Oracle VirtualBox, Windows 10 ISO, and Windows Server 2019 ISO.</p>


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
