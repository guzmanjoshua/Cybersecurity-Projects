# Setup a Home Lab Running Active Directory

# Introduction

Welcome to this comprehensive lab on setting up a home Active Directory environment. This hands-on experience will guide you through the process of creating a small-scale network infrastructure using virtualization technology. You'll learn how to set up a domain controller, configure network services, and manage users - all essential skills for IT professionals working with Windows Server environments.

Throughout this lab, you'll be working with Oracle VirtualBox to create virtual machines, installing and configuring Windows Server 2019 as your domain controller, and setting up a Windows 10 client machine. You'll also gain practical experience with core network services such as DHCP and DNS, as well as learn how to use PowerShell for bulk user creation.

This lab is designed to provide you with a solid foundation in Active Directory management and Windows Server administration. By the end of this tutorial, you'll have a functioning Active Directory environment that mimics a small business network, giving you valuable hands-on experience that can be applied in real-world scenarios.

Let's begin our journey into the world of Active Directory!

# Quick Diagram Overview

<img src="Setup a Home Lab Running Active Directory/ADHL_1.png">

# Section 1: Download Oracle VirtualBox, Windows 10 ISO, and Server 19 ISO

- Use the link to download Oracle VB [Downloads – Oracle VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- Select the platform you are using under VirtualBox Platform Packages and download the VirtualBox Extension Pack
    
    <img src="Setup a Home Lab Running Active Directory/ADHL_2.png">
    
- Use the link to download the Windows Server 2019 ISO [Windows Server 2019 | Microsoft Evaluation Center](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019)
    
   <img src="Setup a Home Lab Running Active Directory/ADHL_3.png">
    
- Use the link to download the Windows 10 installation media [Download Windows 10 (microsoft.com)](https://www.microsoft.com/en-us/software-download/windows10)

<img src="Setup a Home Lab Running Active Directory/ADHL_4.png">

- Open the MediaCreationTool_22H2.exe application once downloaded.
- Press Yes to Allow the application to make changes to your computer.
- Accept the Applicable notices and license terms
- Select ‘Create installation media (USB flash drive, DVD, or ISO file) for another PC’ and click Next.

<img src="Setup a Home Lab Running Active Directory/ADHL_5.png">

- Check on ‘Use the recommended options for this PC’ and click Next.

<img src="Setup a Home Lab Running Active Directory/ADHL_6.png">

- Click on ‘ISO file’ and click Next.

<img src="Setup a Home Lab Running Active Directory/ADHL_7.png">

- Save the ISO file in a place you will remember and in the same place where you downloaded the Windows Server 2019 ISO file.
- Wait for the ISO to finish downloading and click Finish.

# Section 2: Setup and Install Server 19 VM

- Start-up Oracle VM VirtualBox Manager
- Click on New
- Name the VM ‘DC’ for domain controller
- Pick ‘Other Windows (64-bit)’ for the Version
- Click Finish

<img src="Setup a Home Lab Running Active Directory/ADHL_8.png">

- Right click on DC virtual machine and click ‘Settings…’.
- In the ‘General’ section under ‘Advanced’, change ‘Shared Clipboard’ and ‘Drag’n’Drop’ to Bidirectional.

<img src="Setup a Home Lab Running Active Directory/ADHL_9.png">

- In the ‘System’ section under ‘Motherboard’, change the memory size to ‘2048 MB’ if you have 8 MB or more of RAM. If you have less than 8 MB you will want to lower the memory size.

<img src="Setup a Home Lab Running Active Directory/ADHL_10.png">

- In the ‘Processor’ tab change the ‘Processors’ to ‘4’.

<img src="Setup a Home Lab Running Active Directory/ADHL_11.png">

- In the ‘Network’ section, click on Adapter 2.
- Enable Network Adapter and change the ‘Attached to:’ to Internal Network.

<img src="Setup a Home Lab Running Active Directory/ADHL_12.png">

- Click OK on the bottom right corner of the window when finished.
- Start up the Virtual Machine.
- For the DVD selection, select the Windows Server 2019 ISO file you have downloaded and click on ‘Mount and Retry Boot’.

<img src="Setup a Home Lab Running Active Directory/ADHL_13.png">

- Once the Windows Setup window pops up select the language, time, and keyboard input to your choosing.
- Select ‘Install now’
- For the operating system select the Windows Server 2019 Standard Evaluation (Desktop Experience)’.

<img src="Setup a Home Lab Running Active Directory/ADHL_14.png">

- Hit next until you reach ‘Which type of installation do you want?’. Select ‘Custom: Install Windows only (advanced)’.

<img src="Setup a Home Lab Running Active Directory/ADHL_15.png">

- Drive 0 should be selected if so, hit next.
- During the installation the VM should restart. If a command line pops up on the VM DO NOT press any keys.

<img src="Setup a Home Lab Running Active Directory/ADHL_16.png">

- After a while you should see Customize settings. Enter any password you would like but you will need to remember it. Using Password1! is ok since we don’t need to worry about threat actors as much in a sandbox environment.

<img src="Setup a Home Lab Running Active Directory/ADHL_17.png">

- Click finish and log in to the home screen.
- Click on ‘Devices’ located on the top left of the Oracle VirtualBox Windows and click ‘Insert Guest Additions CD Image…’ for a better experience with the VM.

<img src="Setup a Home Lab Running Active Directory/ADHL_18.png">

- Click on file explorer in the VM.
- Click on ‘This PC’ and click on ‘CD Drive (D:) VirtualBox Guest Additions’.
- Click on ‘VBoxWindowsAdditions-amd64’.

<img src="Setup a Home Lab Running Active Directory/ADHL_19.png">

- A window will pop up and you can just click next on everything and hit Install.
- After the install click on, I want to manually reboot later and hit Finish.

<img src="Setup a Home Lab Running Active Directory/ADHL_20.png">

- Click on the windows icon, click the power icon, and click on ‘Shut down’ to avoid any errors.
- Load up the VM again.

# Section 3: Setup IP addressing for Internal NIC

- Click on the network icon on the bottom right of your screen and click on ‘Network’.
    
<img src="Setup a Home Lab Running Active Directory/ADHL_21.png">
    
- Click on ‘Change adapter options’.
- Right click on the first Ethernet, click on Status and then details.
- In this window look for IPv4 Address.
- The IP address should have your home IP address listed.

<img src="Setup a Home Lab Running Active Directory/ADHL_22.png">

- If so, right click on Ethernet again and click on Rename.
- Rename it to _INTERNET_.
- Right click on the second Ethernet and rename it to _INTERNAL_.
- This ethernet is trying to look for a DHCP server but is unsuccessful.
- Right click on _INTERNAL_ and click on Properties.
- Click on the text box of ‘Internet Protocol Version 4 (TCP/IPv4)’.
- Edit the IP address to 172.16.0.1, the Subnet mask to 255.255.255.0, the Default gateway will be empty, and the Preferred DNS server is 127.0.0.1.

<img src="Setup a Home Lab Running Active Directory/ADHL_23.png">

- Click OK for the two windows.
- Close all the windows and right click the windows start menu in the bottom left of your screen
- Select Systems and click Rename this PC.
- Rename it to ‘DC’ for Domain Controller and hit Next, Restart Now, and Continue.

# Section 4: Installing Active Directory Domain Services and Creating a Domain

- Log back in and in the Server Manager Dashboard click on ‘Add roles and features’.

<img src="Setup a Home Lab Running Active Directory/ADHL_24.png">

- Click Next until you reach ‘Server Roles’.
- In ‘Server Roles’ click on the check box for ‘Active Directory Domain Services’.
- Click Add Features and click Next and click Install when you reach ‘Confirmation’.

<img src="Setup a Home Lab Running Active Directory/ADHL_25.png">

- This may take long but once the installation is completed click Close.
- In the top right of Server Manager click on the flag icon with the caution sign.
- Click on ‘Promote this server to a domain controller’.

<img src="Setup a Home Lab Running Active Directory/ADHL_26.png">

- Select the Radio button for ‘Add a new forest’ and change the Root domain name to mydomain.com.
- Click next and for the Password it can just be Password1!.
- Click next until you reached ‘Prerequisites Check’ and click on Install.
- After installation your VM will reset.

<img src="Setup a Home Lab Running Active Directory/ADHL_27.png">

- After your VM resets you should see a new account called MYDOMAIN\Administrator account.
- Log into the MYDOMAIN account.
- On the bottom left of your screen click on the Windows Start icon.
- Click on the ‘Windows Administrative Tools’.

<img src="Setup a Home Lab Running Active Directory/ADHL_28.png">

- Click on ‘Active Directory Users and Computers’.
- Right click on mydoamin.com and click on New and click on Organizational Unit.
- Name it ‘_ADMINS’ and uncheck the box for ‘Protect container from accidental detection’ and click OK.
- Click on the arrow key next to mydomain.com and you should see _ADMINS.
- Right click on _ADMINS and click on New and click on User.

<img src="Setup a Home Lab Running Active Directory/ADHL_29.png">

- For the first and last name it can be your real name. So for me my first name is Joshua and my last name is Guzman.
- For the User logon name, it is going to be ‘a’ for admin, dash symbol ‘-’, the first initial of our first name combine with our last name. So ‘a-jguzman’ for me.
- Click Next.

<img src="Setup a Home Lab Running Active Directory/ADHL_30.png">

- For the password it can just be Password1! again.
- Uncheck the box for ‘User must change password at next logon’.
- Check the box for ‘Password never expires’ and click Next and Finish.

<img src="Setup a Home Lab Running Active Directory/ADHL_31.png">

- Now you should see the account we just created under _ADMINS.
- Right click on the user we created and click on Properties.

<img src="Setup a Home Lab Running Active Directory/ADHL_32.png">

- Click on ‘Member Of’ and click on Add.
- On the text box type in ‘domain admins’ and click on Check Names and click on OK.

<img src="Setup a Home Lab Running Active Directory/ADHL_33.png">

- Click on Apply and OK.
- Go to the windows start icon and sign out.
- Click on Other user and you should see ‘Sign in to: MYDOMAIN’ under Password.
- The username will be the same as we created before. So, a-jguzman for me and password will be Password1!.

# Section 5: Installing Router Access Service and Network Address Translator

- In the Server Manager click on ‘Add roles and features’.
- Click Next until you reach ‘Server Roles’.
- Click on the box for ‘Remote Access’ and click Next.

<img src="Setup a Home Lab Running Active Directory/ADHL_34.png">

- In ‘Role Services’, click on the box for ‘Routing’ and click ‘Add Features’ and click Next.

<img src="Setup a Home Lab Running Active Directory/ADHL_35.png">

- Click next until you reach Confirmation and click Install.
- This will take a while but once completed click Close.
- On the top right of Server Manager click on ‘Tools’ and click on ‘Routing and Remote Access’.

<img src="Setup a Home Lab Running Active Directory/ADHL_36.png">

- Right click on DC (local) and click on ‘Configure and Enable Routing and Remote Access’.
- Click next and select the radio button for ‘Network address translation (NAT)’.

<img src="Setup a Home Lab Running Active Directory/ADHL_37.png">

- Click next and select the radio button for ‘Use this public interface to connect to the internet:’
- If this does not appear just press Cancel close the ‘Routing and Remote Access’ window and reopen ‘Routing and Remote Access’ and ‘Configure and Enable Routing and Remote Access’. This is a common bug.
- Once you are able to select the radio button, click on the ‘_INTERNET_‘ network interface and click Next.

<img src="Setup a Home Lab Running Active Directory/ADHL_38.png">

- Click Finish.
- Close the ‘Routing and Remote Access’ window.

# Section 6: Setup DHCP Server on the Domain Controller

- In Server Manager click on ‘Add roles and features’.
- Click next until you reached ‘Server Roles’ and click on the box for ‘DHCP Server’.
- Click on Add Features and click on next until you reach Confirmation.

<img src="Setup a Home Lab Running Active Directory/ADHL_39.png">

- Click Install and once completed click Close.
- On the top right of Server Manager click on ‘Tools’ and click on ‘DHCP’.
- Click on the arrow key of ‘dc.mydomain.com’.
- Right click on ‘IPv4’ and click on ‘New Scope’.

<img src="Setup a Home Lab Running Active Directory/ADHL_40.png">

- Click Next and for the name we can just put the IP range. So, input ‘172.16.0.100-200’ for the name and click Next.

<img src="Setup a Home Lab Running Active Directory/ADHL_41.png">

- The Start IP address will be 172.16.0.100, the End IP address will be 172.16.0.200, and the Length will be 24.

<img src="Setup a Home Lab Running Active Directory/ADHL_42.png">

- Click Next until you reach the ‘Router (Default Gateway)’ section.
- For the IP address enter 172.16.0.1 and click add.
- Click Next until you see the Finish button and click on Finish.

<img src="Setup a Home Lab Running Active Directory/ADHL_43.png">

- Right click on ‘dc.mydomain.com’ and click on ‘Authorize’.
- Right click on ‘dc.mydomain.com’ again and click on ‘Refresh’.
    
 <img src="Setup a Home Lab Running Active Directory/ADHL_44.png">
    
- The IPs should have a check mark as seen below.

 <img src="Setup a Home Lab Running Active Directory/ADHL_45.png">

# Section 7: Use PowerShell to Add 1k Users

- We are going to download a script for PowerShell using internet explorer. First, we want to disable ‘IE Enhanced Security Configuration’. In a production environment you do not want to do this, but since we are in a sandbox environment it is ok.
- In Server Manager, click on ‘Configure this local server’.

 <img src="Setup a Home Lab Running Active Directory/ADHL_46.png">

- On the right of ‘IE Enhanced Security Configuration’, click on ‘On’.

 <img src="Setup a Home Lab Running Active Directory/ADHL_47.png">

- For ‘Administrators:’ and ‘Users:’ select ‘Off’ and click OK.

 <img src="Setup a Home Lab Running Active Directory/ADHL_48.png">

- In the VM open up internet explorer and click OK if you receive the below window pop up.

 <img src="Setup a Home Lab Running Active Directory/ADHL_49.png">

- You can either copy this link and paste it in the URL in the VM in internet explorer and download the file.
- Or download the file on your mmain computer, unzip it, and move it to the VM.
[Joshua Guzman - AD PS Zip File](https://github.com/guzmanjoshua/Cybersecurity-Projects/blob/main/Setup%20a%20Home%20Lab%20Running%20Active%20Directory/AD_PS-master.zip)

- To get a detail explanation of the Script you can go to my other github link.
[Joshua Guzman - Explanation of the 1000 Users Script (1_CREATE_USERS) Github](https://github.com/guzmanjoshua/Cybersecurity-Projects/blob/main/Explanation%20of%20the%201000%20Users%20Script%20(1_CREATE_USERS).md)

- Unzip the file to C:\users\a-jguzman(your admin account name)\desktop\AD_PS-master.
- Open the file and open the text file names.txt.

 <img src="Setup a Home Lab Running Active Directory/ADHL_50.png">

- Enter you first and last name on top of the text document.
- Save and close the text file.
- Click on the windows start icon and right click on Windows PowerShell ISE.
- Click on ‘More’ and click on ‘Run as administrator’.

 <img src="Setup a Home Lab Running Active Directory/ADHL_51.png">

- Click Yes and on top of the PowerShell window click on the open file icon which is called ‘Open Script’.

 <img src="Setup a Home Lab Running Active Directory/ADHL_52.png">

- Navigate the file explorer to where you downloaded the unzip file and click on ‘1_CREATE_USERS’.
- In the PowerShell terminal type in ‘Set-ExecutionPolicy Unrestricted’, press Enter and click ‘Yes to All’. In a production environment you don’t want to do this. We are doing this in our lab to bypass the requirement to have the file digitally signed.

 <img src="Setup a Home Lab Running Active Directory/ADHL_53.png">

- In the PowerShell terminal type in ‘cd C:\users\a-jguzman\desktop\AD_PS-master’ and press Enter.

 <img src="Setup a Home Lab Running Active Directory/ADHL_54.png">

- On the top of the PowerShell window click on the green play button.

 <img src="Setup a Home Lab Running Active Directory/ADHL_55.png">

- Click on ‘Run once’ and let it create all the users. If you get errors don’t worry about it since its just duplicate names in the text document.
- It may take a while but once finish open up ‘Active Directory Users and Computers’.
- Expand mydomain.com. Click on _USERS and you will see all the users that was just created.

 <img src="Setup a Home Lab Running Active Directory/ADHL_56.png">

- To find the users by name you can right click on mydomain.com, click on Find, you can type in the names on Name and click on ‘Find Now’.

# Section 8: Create and Setup Windows 10 VM

- You can now minimize the DC VM and open Oracle VM VirtualBox Manager.
- Click on New, name the VM to CLIENT1.
- Pick Windows 10 (64-bit) for the Version and click Finish.

 <img src="Setup a Home Lab Running Active Directory/ADHL_57.png">

- Right click on CLIENT1 virtual machine and click ‘Settings…’.
- In the ‘General’ section under ‘Advanced’, change ‘Shared Clipboard’ and ‘Drag’n’Drop’ to Bidirectional.
- In the ‘System’ section under ‘Motherboard’, change the memory size to ‘2048 MB’ if you have 8 MB or more of RAM. If you have less than 8 MB you will want to lower the memory size.
- In the ‘Processor’ tab change the ‘Processors’ to ‘4’.
- In the ‘Network’ section, under Adapter 1 change the ‘Attached to:’ to Internal Network.
- Click OK on the bottom right corner of the window when finished.
- Start and open the CLIENT1 VM.
- For the DVD selection, select the Windows 10 ISO file you have downloaded earlier and click on ‘Mount and Retry Boot’.
- Once the Windows Setup window pops up select the language, time, and keyboard input to your choosing and once done Install Now.
- Click on ‘I don’t have a product key’ and select ‘Windows 10 Pro’ and click Next.

 <img src="Setup a Home Lab Running Active Directory/ADHL_58.png">

 <img src="Setup a Home Lab Running Active Directory/ADHL_59.png">

- Click on the check box for ‘I accept the license terms’ and click Next.
- Click on ‘Custom: Install Windows only (advanced)’.
- Drive 0 Unallocated Space should be selected, click on Next.
- Wait for the install and once you are in the user configurations click Yes, Yes, and skip.
- When you reach to the ‘Let’s connect you to a network’ page click on the ‘I don’t have internet’ on the bottom left of the page. If it does pop up.
- Click on ‘Continue with limited setup’. If it does pop up.
- On ‘How would you like to set up:’ click on ‘Set up for an organization’ and click Next.

 <img src="Setup a Home Lab Running Active Directory/ADHL_60.png">

- Click on ‘Domain join instead’.

 <img src="Setup a Home Lab Running Active Directory/ADHL_61.png">

- In the ‘Who’s going to use this PC?’ page you can enter the name ‘user’ and click Next.

 <img src="Setup a Home Lab Running Active Directory/ADHL_62.png">

- Do not enter a password, just click Next.
- In this page click Not now

 <img src="Setup a Home Lab Running Active Directory/ADHL_63.png">

- In the ‘Choose privacy settings for your device’ page turn all of the options off and click Accept once finished.

 <img src="Setup a Home Lab Running Active Directory/ADHL_64.png">

- Click Skip on this page.

 <img src="Setup a Home Lab Running Active Directory/ADHL_65.png">

- Click on ‘Not Now’ for the Cortana page.

 <img src="Setup a Home Lab Running Active Directory/ADHL_66.png">

- Wait until the installation is finished to move on to the next step.

# Section 9: Connect Windows 10 VM to the Domain Controller

- Right click the Windows start icon and click on ‘System’.

 <img src="Setup a Home Lab Running Active Directory/ADHL_67.png">

- Scroll down on the right side of the Settings window until you see ‘Rename this PC (advanced)’ and click on it.

 <img src="Setup a Home Lab Running Active Directory/ADHL_68.png">

- Click on ‘Change’.

 <img src="Setup a Home Lab Running Active Directory/ADHL_69.png">

- Rename it to CLIENT1 under the ‘Computer name:’ text field.
- Click on the ‘Domain:’ radio button and type in ‘mydomain.com’ and click OK.

 <img src="Setup a Home Lab Running Active Directory/ADHL_70.png">

- The username will be the admin account we created, so for me it’s ‘a-jguzman’ and the password is Password1!. After that click OK to everything and click ‘Close’ on the ‘System Properties’ window.

 <img src="Setup a Home Lab Running Active Directory/ADHL_71.png">

- Click on Restart Now once the window pops up.

 <img src="Setup a Home Lab Running Active Directory/ADHL_72.png">

- Open up the DC VM and open up the ‘DHCP’ window under ‘Tools’ in Server Manager.

 <img src="Setup a Home Lab Running Active Directory/ADHL_73.png">

- Click on ‘Address Leases’ which is under ‘dc.mydomian.com’ > ‘IPv4’ > ‘Scope [172.16.0.0] 172.16.0.100-200’.
- You can see the CLENT1 VM we just made and renamed is now connected to the Domain Controller and the DHCP server gave it an address.
    
   <img src="Setup a Home Lab Running Active Directory/ADHL_74.png">
    
- If you go back to the CLIENT1 VM and go back to the sign in page and click on ‘Other user’. You will see it now sign in to MYDOMAIN.

 <img src="Setup a Home Lab Running Active Directory/ADHL_75.png">

# Conclusion

Congratulations on completing this comprehensive lab on setting up a home Active Directory environment! Throughout this hands-on experience, you've gained valuable skills that are essential for IT professionals working with Windows Server environments.

You've successfully accomplished several key tasks:

- Set up a domain controller using Windows Server 2019
- Configured core network services such as DHCP and DNS
- Created and managed users, including bulk user creation with PowerShell
- Installed and configured Remote Access Service and Network Address Translator
- Set up a Windows 10 client machine and connected it to the domain

This lab has provided you with practical experience in creating a small-scale network infrastructure using virtualization technology. The skills you've developed here are directly applicable to real-world scenarios in enterprise environments.

As you continue your journey in IT, remember that Active Directory is a fundamental component of many organizations' network infrastructures. The knowledge you've gained here will serve as a solid foundation for more advanced topics in network administration and security.

Keep practicing and exploring new features and configurations to further enhance your skills. Good luck in your future endeavors in the world of IT!
