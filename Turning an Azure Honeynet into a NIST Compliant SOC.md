# Turning an Azure Honeynet into a NIST Compliant SOC with Microsoft Sentinel and Custom Workbooks

Creating a Honeynet in Azure allows for Threat Intelligence gathering. Which can enhance the ability of the Security Operations Center (SOC) to track and identify threat actors’ potential location, the techniques, tactics, and procedures they use, and what kind of exploits are currently being executed. I will be showcasing how I used Azure to create a Honeynet, detailing how I configured the Virtual Machines to receive remote access connections directly from the internet. And then, how I secured them within 24 hours using NIST 800-53 Rev. 5 guidelines. Threat Intelligence and Compliance are only two pieces of the Cyber Security puzzle, but these things help to ensure the availability, integrity, confidentiality, and privacy of cloud systems and data.

## Section 1 - Creating the Windows and Linux Virtual Machine

WindowsVM Creation: **STEP 1**

- First, at the home page click on “Virtual machines.”

![1.2 VM.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/1.2_VM.png)

WindowsVM Creation: **STEP 2**

- Click on the dropdown arrow.

![Untitled](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/Untitled.png)

WindowsVM Creation: **STEP 3**

- Click on ‘Azure Virtual machine’.

![Screenshot 2024-02-07 13130G9.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/Screenshot_2024-02-07_13130G9.png)

WindowsVM Creation: **STEP 4**

- Create a new resource group by clicking ‘create new’.
- In my lab I named the resource group ‘RG-SOC’. RG is the initials for ‘Resource Group’. This is a great naming convention to quickly understand what the object is.
- For future VMs created under the same resource groups, just click the dropdown.

![Untitled](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/Untitled%201.png)

WindowsVM Creation: **STEP 5**

- Name the Virtual Machine. I named mines WindowsVM for windows virtual machine and LinuxVM for Linux virtual machine.
- For the Region, pick what is right for you.

![5 VM.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/5_VM.png)

WindowsVM Creation: **STEP 6**

- For Availability zone I chose ‘No infrastructure redundancy required’.

![Untitled](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/Untitled%202.png)

WindowsVM Creation: **STEP 7**

- The Image is the operating system (OS) or application for the VM.
- For my VM I chose ‘Windows 10 Pro, version 22H2 - x64 Gen2’ and ‘Ubuntu Server 20.04 LTS - x64 Gen2’ for my second VM.

![Untitled](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/Untitled%203.png)

WindowsVM Creation: **STEP 8**

- The Size it the specs of the VM. So if you doing a heavy task on the VM, higher specs is recommended. Beware, that specs will contribute to your cost per month.
- Enter your username and password for the admin account. You must remember the Username and Password.
- Check the box of the ‘I confirm I have an eligible Windows 10/11 license with multi-tenant hosting rights.’
- Once finish, skip to Networking.

![Untitled](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/Untitled%204.png)

WindowsVM Creation: **STEP 9**

- Select Basic for NIC network security group.
- Select the check box for Delete public IP and NIC when VM is deleted.
- De-select the Enable accelerated networking.
- Once finish, press Review + Create button.

![9.1.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/9.1.png)

WindowsVM Creation: **STEP 10**

- Press the Create button.

![10.1 VM.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/10.1_VM.png)

## Section 2 - Edit the Firewall

Network Security Group: **STEP 1**

- Once the VM is finished, go to the search tab and search for ‘Network security groups’.
- Click on Network security groups.

![NSG 1.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/NSG_1.png)

Network Security Group: **STEP 2**

- Click on the WindowsVM that was just created.

![NSG 2.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/NSG_2.png)

Network Security Group: **STEP 3**

- Click on Inbound security rules.

![NSG 3.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/NSG_3.png)

Network Security Group: **STEP 4**

- Click on Add.

![NSG 4.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/NSG_4.png)

Network Security Group: **STEP 5**

- Change Destination port ranges to *, which indicates all.
- Have the Description describe what is happening, for example “Everything is open and allowed!”.
- When finished click on Save.
- Repeat the steps for the LinuxVM that was just created.

![NSG 5.2.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/NSG_5.2.png)

## Section 3 - Connecting to the VMs

To edit security rules in the Windows Virtual Machine, we must connect to it. In the following steps, I will demonstrate how to connect to a WindowsVM. In addition, I will demonstrate how to connect to a LinuxVM.

Connect to WindowsVM: **STEP 1**

- To connect to our WindowsVM, we need to obtain the public IP address.
- To do so go to the top of the search bar.
- Type in “Virtual machine”
- Click on the Virtual machine text under Services.

![CTVM 1.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/CTVM_1.png)

Connect to WindowsVM: **STEP 2**

- Search for the name of your WindowsVM
- Look under Public IP address to get the address.

![CTVM 2.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/CTVM_2.png)

Connect to WindowsVM: **STEP 3**

- On your Windows computer press on the search icon on the taskbar.
- On the search bar type in “Remote Desktop Connection”.
- Click on Remote Desktop Connection under Best match.
- Click on Open.

![CTVM 3.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/CTVM_3.png)

Connect to WindowsVM: **STEP 4**

- Input your WindowsVM public IP address on the Computer text box
- When finish press Connect.

![CTVM 4.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/CTVM_4.png)

Connect to LinuxVM: **STEP 1**

- To connect to our LinuxVM, we need to obtain the public IP address.
- Like in “Connect to WindowsVM: **STEP 2**” ****go to your virtual machines.
- Search for the name of your LinuxVM.
- Look under Public IP address to get the address.

![Untitled](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/5a7df315-6564-423e-aca5-f231a609603d.png)

Connect to LinuxVM: **STEP 2**

- On your Windows computer press on the search icon on the taskbar.
- On the search bar type in “Windows Powershell”.
- Click on Run as Administrator.

![a linux4.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/a_linux4.png)

Connect to LinuxVM: **STEP 3**

- In the Powershell type in “ssh analyst@(your linux public ip address)”.
- Press Enter.
- You will be prompted to enter your Linux password if successful.
- If so, type in your Linux password. NOTE: Your input will not show to prevent shoulder surfing.

![Untitled](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/Untitled%205.png)

- If successful a prompt like this will appear and the connection is complete.

![Untitled](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/37b1b5f2-755e-4ada-a2dd-ffcdad25a17c.png)

## Section 4: Editing Windows Defender Firewall Rules in WindowsVM

To make our WindowsVM more vulnerable we must connect to it and edit its Windows Defender Firewall settings internally.

Edit wf.msc in WindowsVM: **STEP 1**

- Connect to your WindowsVM as demonstrated in “Section 3: Connecting to the VMs”.
- On your WindowsVM press on the search icon on the taskbar.
- On the search bar type in “wf.msc”.
- Click on Run as Administrator.

![a edit.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/a_edit.png)

Edit wf.msc in WindowsVM: **STEP 2**

- Click on “Windows Defender Firewall Properties”.

![a edit 1.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/a_edit_1.png)

Edit wf.msc in WindowsVM: **STEP 3**

- In the Domain Profile turn the Firewall state Off.

![a edit 2.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/a_edit_2.png)

Edit wf.msc in WindowsVM: **STEP 4**

- In the Private Profile turn the Firewall state Off.

![a edit 3.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/a_edit_3.png)

Edit wf.msc in WindowsVM: **STEP 5**

- In the Public Profile turn the Firewall state Off.
- When done press Apply and then press OK.

![a edit 4.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/a_edit_4.png)

## Section 5: Implementing Incidents and Alerts List

Implementing an Incidents and Alerts list in the Azure honeynet is crucial for maintaining security and managing risk. This list will provide real-time notifications of potential security breaches and unusual activities, allowing the security team to respond promptly. It also facilitates tracking of incidents, helping to identify patterns and trends in security threats. This can inform improvements in security measures and strategies over time. In the following steps, I will demonstrate how to implement incidents and alerts.

Implementing Incidents and Alerts: **STEP 1**

- In the top search bar type in “Microsoft Sentinel”.
- Click on the Microsoft Sentinel text that is under Services.

![IC 1.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/IC_1.png)

Implementing Incidents and Alerts: **STEP 2**

- Click on your Log Analytics workspace, in my case its “LAW-SOC”.

![IC 2.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/IC_2.png)

Implementing Incidents and Alerts: **STEP 3**

- Click on Analytics.

![IC 3.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/IC_3.png)

Implementing Incidents and Alerts: **STEP 4**

- Press on the Create button.
- Click on Scheduled query rule.
- If you have Incidents and Alerts files saved, you can press the Import button.

![IC 4.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/IC_4.png)

To view the KQL, description, MITRE ATT&CK, response actions, and containment and recovery of my Incidents and Alerts list, click the link on the right: [Joshua Guzman - 13 Microsoft Azure Incidents i.e. (Alerts) GitHub Project](https://github.com/guzmanjoshua/Incident-Report/blob/main/Incidents%20Home%20Page.md)

## Section 6: Implementing Workbooks and Geo Locations

Workbooks and Geo Locations are crucial for building an Azure honeynet. Workbooks help to organize, visualize, and analyze data collected by Azure Monitor Logs. They allow you to create rich visual reports within the Azure portal. These reports can be used to monitor the performance and availability of applications, infrastructure, and network.

Geo Locations, on the other hand, provide a geographical context to the data. They allow you to visualize where the threats are coming from, which can help to identify patterns, trends, and potential threat actors' locations. This information can be invaluable in enhancing the security strategy, enabling quicker response to threats, and providing a broader context for threat intelligence.

In an Azure honeynet, these features combined can provide a powerful tool for threat detection and response. They allow for real-time tracking and analysis of potential security breaches and unusual activities, enabling the security team to respond promptly and effectively. In the following steps, I will demonstrate how to implement Workbooks and Geo Locations.

Implementing Workbooks and Geo Locations: **STEP 1**

- Repeat steps 1 and 2 of Implementing Incidents and Alerts.
- So, navigate to Microsoft Sentinel and your Log Analytics workspace.

![IC 1.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/IC_1%201.png)

![IC 2.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/IC_2%201.png)

Implementing Workbooks and Geo Locations: **STEP 2**

- Click on Workbooks on the left list.

![MSWHN 1.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/MSWHN_1.png)

Implementing Workbooks and Geo Locations: **STEP 3**

- Click on Add Workbook

![MSWHN 2.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/MSWHN_2.png)

Implementing Workbooks and Geo Locations: **STEP 4**

- Click on Edit

![MSWHN 3.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/MSWHN_3.png)

Implementing Workbooks and Geo Locations: **STEP 5**

- Click on the three dots icon on both of the default content.
- Click on the Remove button.

![MSWHN 4.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/MSWHN_4.png)

Implementing Workbooks and Geo Locations: **STEP 6**

- Click on the Add button.
- Click on the Add query button.

![MSWHN 5.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/MSWHN_5.png)

Implementing Workbooks and Geo Locations: **STEP 7**

- Click on the Time Range dropdown button.
- Select Last 30 days.

![MSWHN 6.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/MSWHN_6.png)

Implementing Workbooks and Geo Locations: **STEP 8**

- Click on the Size dropdown button.
- Select Full.

![MSWHN 8.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/MSWHN_8.png)

Implementing Workbooks and Geo Locations: **STEP 9**

- Click on the empty space in the middle of the screen to enter in your KQL.
- When finished, click on Done Editing under the KQL box.

![MSWHN 9.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/MSWHN_9.png)

Implementing Workbooks and Geo Locations: **STEP 10**

- Click on Done Editing on top of the screen.
- Click on the floppy disk save icon.

![MSWHN 10.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/MSWHN_10.png)

Implementing Workbooks and Geo Locations: **STEP 11**

- Fill in the information in the Save As box.
- When finish click on the Apply button.

![MSWHN 11.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/MSWHN_11.png)

To view the KQL, and Geo Locations of my Geo Location Workbooks, click the link on the right: [Joshua Guzman - Workbooks and Geo Locations](https://github.com/guzmanjoshua/Cybersecurity-Projects/blob/main/Workbooks%20and%20Geo%20Locations.md)

## Section 7: Run the Honeynet for 24 Hours Unsecured

Having made our honeynet unsecured and implemented security information through workbooks, and incidents list we can now run our VMs.

Run Honeynet Unsecured for 24 Hours: **STEP 1**

- Run your VM honeynet(s) for 24 hours as is unsecured.

Run Honeynet Unsecured for 24 Hours: **STEP 2**

- After the 24 hours, stop your VM honeynet(s).
- Continue to Section 8.

## Section 8: Recording Results from KQL Queries

Utilizing KQL Queries is instrumental in collecting and analyzing data from our VMs run both unsecured and secured over a 48-hour period. These logs allow us to delve deep into the behavioral patterns and gather crucial insights about the system. When the system is unsecured, we can identify potential threats, their frequency, and their sources. Upon securing the system, we can monitor how these threats are mitigated and how the security measures in place are performing. This comparison provides valuable information about the effectiveness of our security measures. Additionally, KQL Queries enable us to detect anomalies, identify potential areas of improvement, and ultimately strengthen our security posture. In the following steps, I will demonstrate how to use KQL Queries.

KQL Queries: **STEP 1**

- On top of the search bar type in “Monitor”.
- Click on the Monitor text that is under Services.

![ML 1.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/ML_1.png)

KQL Queries: **STEP 2**

- Click on the Logs text on the left list.

![ML 2.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/ML_2.png)

KQL Queries: **STEP 3**

- On the top left of the Logs check if the appropriate Log Analytics workspace is selected
- If not click on the Select scope button.

![ML 3.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/ML_3.png)

KQL Queries: **STEP 4**

- On the Select a scope box navigate through the list to find the appropriate Log Analytics workspace.
- You may need to press on the dropdown arrows to view the full list.
- When you find the appropriate Log Analytics workspace click on its checkmark box.
- When done press the Apply button.

![ML 4.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/ML_4.png)

KQL Queries: **STEP 5**

- In the middle of the text box is where you input the KQLs
- Once you finish inputting the KQL, press the Run button.
- Below the KQL text box is where you can the results

![ML 5.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/ML_5.png)

KQL Queries: **STEP 6**

- Use the Helper Queries listed below to help you gather the information.

![Helper Queries.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/Helper_Queries.png)

KQL Queries: **STEP 7**

- Create a Windows Excel to organize and compare your results.
- Use my results below to give you an example on how to organize the data.
- Move on to Section 9 and further sections to make the honeynet secure, run it for 24 hours, and record the results.

![Before And After.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/Before_And_After.png)

## Section 9: Implementing NIST SP 800-53 Rev. 5

Implementing NIST SP 800-53 Rev. 5 is a crucial part of building an Azure Honeynet due to its comprehensive guidelines for managing and maintaining a robust security system. This framework offers a holistic approach to managing cybersecurity risk, encompassing a wide array of security controls across various areas such as access control, audit and accountability, system and information integrity, and more.

Adhering to NIST SP 800-53 standards ensures that the Honeynet adheres to approved security practices and is equipped with rigorous mechanisms to detect, prevent, and mitigate potential security breaches. This is particularly important in a Honeynet environment, where the network is designed to be attractive to potential attackers.

Furthermore, implementing NIST SP 800-53 Rev. 5 enhances the credibility and trustworthiness of the Honeynet. Should a security breach occur, the organization can demonstrate that it had implemented comprehensive and state-of-the-art security controls, potentially limiting legal and reputational damage.

Overall, integrating NIST SP 800-53 Rev. 5 into an Azure Honeynet aids in maintaining a secure, resilient, and trusted network environment, ultimately protecting valuable data and systems from threats.

To learn how to implement NIST SP 800-53 Rev. 5, click the link on the right: [Cybersecurity-Projects/NIST SP 800-53 Rev 5 in Azure.md at main · guzmanjoshua/Cybersecurity-Projects (github.com)](https://github.com/guzmanjoshua/Cybersecurity-Projects/blob/main/NIST%20SP%20800-53%20Rev%205%20in%20Azure.md)

## Section 10: Secure the Environment

To make the environment secure we must revisit Section 2 and Section 4.

Revisiting Section 2: **STEP 1**

- Go back to the Network Security Group.
- On both of the LinuxVM and WindowsVM delete the “DANGERAllowAnyCustomAnyInbound” inbound security rule by pressing the trash icon.

![a secure 1.1.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/a_secure_1.1.png)

Revisiting Section 4: **STEP 1**

- Connect to your WindowsVM.
- Revisit the wf.msc Windows Defender Firewall Properties.
- Turn the Firewall State on for the Domain, Private, and Public Profile.
- When finish click on the Apply button then the OK button.

![a edit.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/a_edit%201.png)

![a edit 1.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/a_edit_1%201.png)

![a edit 2.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/a_edit_2%201.png)

![a edit 3.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/a_edit_3%201.png)

![a edit 4.png](Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SO%20e347942d754641b7ac914de3898d9922/a_edit_4%201.png)

## Section 11: Run the Honeynet for 24 hours Secured

Run Honeynet Secured for 24 Hours: **STEP 1**

- Run your VM honeynet(s) for 24 hours.

 

Run Honeynet Secured for 24 Hours: **STEP 2**

- After the 24 hours, stop your VM honeynet(s).
- Repeat Section 8 to finalize your results.

# Conclusion

This lab offers a comprehensive exploration of setting up and managing an Azure Honeynet, providing a practical illustration of the importance of robust cybersecurity measures. By observing the behavior of the system in both unsecured and secured states, we gain vital insights into potential threats, their frequency, and their sources. The transformation from an unsecured to a secured state offers a valuable comparison, underscoring the effectiveness of implemented security measures.

The exercise further emphasized the importance of continuous monitoring, adjustment of security protocols, and the role of tools such as KQL Queries in collecting and analyzing data for improved security. A significant learning from this lab is that threat landscapes are continuously evolving, necessitating an equally dynamic and proactive approach to cybersecurity.

In conclusion, the knowledge and skills acquired from this lab are invaluable in the real-world application of cybersecurity principles. The hands-on experience with Azure Honeynet not only aids in understanding the theoretical aspects of cybersecurity but also equips individuals to handle practical scenarios, enhancing overall security posture.