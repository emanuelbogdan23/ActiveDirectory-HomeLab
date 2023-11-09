<h1>Active Directory - Home Lab</h1>

<h2>Description</h2>
In this tutorial, we look into real-time cyber threat detection and response. I will set up Azure Sentinel, a powerful Security Information and Event Management (SIEM) solution, and connected it to a live virtual machine that's masquerading as a honey pot, enticing would-be attackers.

What makes this project fun is that we will be witnessing live attacks—specifically, Remote Desktop Protocol (RDP) brute force attempts—from all corners of the globe. Additionally, we will leverage a PowerShell script to extract the geographical information of these attackers, and then we'll visualize their locations on the Azure Sentinel Map.
<br />


<h2>Languages and Utilities Used</h2>

- <b>PowerShell</b> 
- <b>Oracle Virtual Box</b>

<h2>Environments Used </h2>

- <b>Windows 10</b> (21H2)
- <b>Server 2019</b>

<h2>Project walk-through:</h2>

<p align="center">
The network diagram illustrates the Active Directory Home Lab's structure. It features a Domain Controller (DC), a Client Machine, and two network connections: Internal (for internal communication) and External (providing internet access and routing for private network clients). This visual aids in understanding the project's network configuration. <br/>
<br />
<img src="https://imgur.com/UGgLac7.png" height="80%" width="80%" alt="Network Diagram"/>
<br />
<br />

 
<p align="center">The first phase involved downloading essential components: Oracle VirtualBox, Windows 10 ISO, and Windows Server 2019 ISO.
From there, I created two virtual machines (VMs) within Oracle VirtualBox, a Domain Controller (DC) and a Client Machine. 
</p>
<br />


<div style="display: flex; align="center">
  <div style="flex: 1;">
    <h3>Setting Up Network Interface Cards (NICs)</h3>
    <ul style="list-style-type: disc; margin: 10px 0 10px 20px;">
      <li>Began by configuring the Network Interface Cards (NICs).</li>
      <li>Two NICs in the network diagram: one for internet connectivity and another for the internal network.</li>
      <li>The NIC dedicated to the internet automatically obtained an IP address from the home router.</li>
      <li>Renamed the NIC for the internal network to "Internal" for clarity in network management.</li>
      <li>Configured the IP addressing for the internal NIC:</li>
      <ul style="list-style-type: circle; margin: 5px 0 5px 20px;">
        <li>Set its IP address to 172.16.0.X, allowing flexibility in choosing X within the range.</li>
        <li>Defined the subnet mask as 255.255.255.0.</li>
        <li>Left the default gateway blank, as the Domain Controller would serve as the gateway.</li>
        <li>Opted for DNS settings using either the Domain Controller's IP address or the loopback address (127.0.0.1), ensuring reliance on the internal network for DNS services.</li>
      </ul>
      <li>These initial steps were pivotal to establish a strong network foundation.</li>
    </ul>
  </div>
<br />
  <div align="center">
    <img src="https://imgur.com/7mhaUQe.png" height="50%" width="50%" alt="Internal and External NICs with Internal IPv4 Properties"/>
  </div>
</div>
<br />


<h3>Installing Active Directory Domain Services (AD DS)</h3>
<p>The next significant step was the installation of Active Directory Domain Services (AD DS):</p>
<ul>  
  <li><strong>Post-Deployment Configuration:</strong> After successfully installing AD DS, I addressed the post-deployment configuration, which included promoting the computer to a domain controller.</li>
  <li><strong>Creating the Domain:</strong> During the domain creation process I used "mydomain.com" to create the domain structure. A secure password was set for the Directory Services Restore Mode.</li>
  <li><strong>Account Management:</strong> I replaced the built-in administrator account with a dedicated domain admin account to enhance security. This new account was created in the Active Directory Users and Computers tool, assigned to the admins Organizational Unit, and configured with the appropriate permissions.</li>
</ul>
<br />
<div align="center">
 <img src="https://imgur.com/3RssOoc.png" height="90%" width="90%" alt="Deployment Configuration and Admin User Set Up"/>
</div>
<br />


<h3>Installing RAS (Remote Access Server) and NAT (Network Address Translation)</h3>
<p>In this stage, I focused on enhancing network capabilities by installing RAS (Remote Access Server) and NAT (Network Address Translation) on the domain controller. This setup allows the Windows 10 clients to reside on a private virtual network while still being able to access the internet through the domain controller. This was achieved through the following steps:</p>
<ol>
  <li>Opened the "Add Roles and Features" section in Windows Server.</li>
  <li>Specified the server for installing RAS and NAT.</li>
  <li>Selected the "Remote Access" role along with other components.</li>
  <li>Installed routing and enabled NAT to facilitate internal clients' internet access.</li>
  <li>Configured NAT settings to ensure the correct interface for internet connectivity.</li>
</ol>
<p>This configuration completes another vital component of the network infrastructure and aligns with our network diagram's objectives.</p>
<br />
<div align="center">
 <img src="https://imgur.com/qROODSm.png" height="70%" width="70%" alt="Deployment Configuration and Admin User Set Up"/>
</div>
<br />


<h3>Setting Up DHCP for Seamless Connectivity</h3>
<p>Next, I established a DHCP (Dynamic Host Configuration Protocol) server on the domain controller to automate IP address allocation. My primary objective was to provide the Windows 10 clients with internet access while integrating them into a private internal network, resembling typical office or school environments.</p>
<ul>
  <li><strong>Accessing Roles and Features</strong>: I initiated the process by navigating to the "Add Roles and Features" section in Windows Server on the domain controller, preparing for the installation of the DHCP role and associated components.</li>
  <li><strong>Configuring the DHCP Role</strong>: After selecting the DHCP role and including necessary features, I ensured the DHCP server's proper operation.</li>
  <li><strong>Defining the DHCP Scope</strong>: I defined a DHCP scope specifying the IP address range (from 172.16.0.100 to 172.16.0.200) and a subnet mask (/24) for automatic IP assignment to client devices.</li>
  <li><strong>No Exclusions</strong>: To maintain flexibility in IP assignments, I chose not to exclude specific IP addresses from the scope.</li>
  <li><strong>Lease Duration</strong>: Considering lab requirements and efficient scope management, I set the lease duration for IP addresses to 8 days.</li>
  <li><strong>Configuring DHCP Options</strong>: Vital information such as the default gateway and DNS server was configured as DHCP options to be provided to client devices.</li>
  <li><strong>Activation of DHCP Scope</strong>: The DHCP scope was activated, rendering the DHCP server ready to allocate IP addresses to clients.</li>
</ul>
<p>This pivotal step ensures seamless internet connectivity for our Windows 10 clients while they operate within our private network. It aligns perfectly with the objectives set forth in our network diagram, significantly enhancing the overall functionality and resilience of our lab environment.</p>
<div align="center">
 <img src="https://imgur.com/8GeBvRi.png" height="90%" width="90%" alt="Deployment Configuration and Admin User Set Up"/>
</div>
<br />
<br />


<h3>Generating Sample Users with PowerShell Script</h3>
<p>In this stage, I generated 1000 sample users in Active Directory efficiently using a PowerShell script.</p>
<br />
<ul>
  <li><strong>Script Overview:</strong> The PowerShell script in the lab provides an efficient means of automating user account creation in Active Directory. It imports the requisite Active Directory module and orchestrates the generation of uniform passwords for user accounts. The script sources its list of randomized first and last names from a text file and adroitly combines them to create unique usernames. The process involves the creation of individual user accounts in Active Directory, encompassing details like passwords, display names, and other pertinent attributes. Additionally, it establishes a new Organizational Unit (OU) to house the newly generated user accounts.</li>
  <li><strong>Running the Script:</strong> I executed the PowerShell script, importing the Active Directory module, and initiated the user creation process. It successfully created 1000 user accounts.</li>
  <li><strong>User Account Verification:</strong> To ensure the effectiveness of the script, I verified that the user accounts were created in Active Directory. I also confirmed the presence of the users' Organizational Unit (OU) in Active Directory.</li>
</ul>
<div align="center">
 <img src="https://imgur.com/Dl6oNrv.png" height="90%" width="90%" alt="Deployment Configuration and Admin User Set Up"/>
</div>
<br />
<br />


<h3>Setting Up Windows 10 VM with Internal Network Interface</h3>
<p>To expand the network, I created a Windows 10 VM in Oracle VirtualBox. It was configured to use an internal network interface to connect to our internal corporate network, distinct from the external internet network.</p>
<ul>
  <li><strong>VM Creation:</strong> Configured VM settings by selecting "Internal Network Adapter" for the VM to ensure it's isolated from the external network but can communicate within the internal network.</li>
  <li><strong>Windows 10 Installation:</strong> Attached the Windows 10 ISO and installed Windows 10 as a local account to facilitate domain joining later.</li>
  <li><strong>Joining Corporate Domain:</strong> Joined the corporate domain ("mydomain.com") using domain credentials.</li>
  <li><strong>Final Network Testing:</strong> Verified network connectivity, IP configuration, DNS resolution, and access to network resources.</li>
</ul>
<p>This addition completes the lab's network infrastructure, mirroring a real corporate environment.</p>
<div align="center">
 <img src="https://imgur.com/Af8SVfa.png" height="90%" width="90%" alt="Deployment Configuration and Admin User Set Up"/>
</div>
<br />
<br />


<h3>Key Learnings</h3>
<ul>
  <li><strong>Active Directory Mastery:</strong> I acquired in-depth knowledge of Active Directory, mastering domain controllers, user management, and domain configuration.</li>
  <li><strong>Network Proficiency:</strong> I became proficient in network setup, including IP addressing, NIC configuration, and DHCP server implementation.</li>
  <li><strong>Network Architecture:</strong> The project's network diagram deepened my grasp of network infrastructure, especially internal and external connections.</li>
  <li><strong>Scripting Efficiency:</strong> Utilizing PowerShell for user automation revealed the power of scripting in simplifying repetitive tasks.</li>
  <li><strong>Virtualization Skills:</strong> I honed skills in virtual machine setup and management using Oracle VirtualBox.</li>
  <li><strong>DHCP Automation:</strong> Implementing a DHCP server provided insights into automating IP address allocation for seamless network connectivity.</li>
</ul>
<br />


<h4>Acknowledgment</h4>
<p>I would like to express my gratitude to Josh Madakor, whose instructional video on YouTube (available at <a href="https://www.youtube.com/watch?v=MHsI8hJmggI&ab_channel=JoshMadakor">this link</a>) provided invaluable guidance and insights. The knowledge I gained from his video was instrumental in the successful execution of this project.</p>
<br />
<br />




