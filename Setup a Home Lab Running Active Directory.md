# Setup a Home Lab Running Active Directory

# Introduction

Welcome to this comprehensive lab on setting up a home Active Directory environment. This hands-on experience will guide you through the process of creating a small-scale network infrastructure using virtualization technology. You'll learn how to set up a domain controller, configure network services, and manage users - all essential skills for IT professionals working with Windows Server environments.

Throughout this lab, you'll be working with Oracle VirtualBox to create virtual machines, installing and configuring Windows Server 2019 as your domain controller, and setting up a Windows 10 client machine. You'll also gain practical experience with core network services such as DHCP and DNS, as well as learn how to use PowerShell for bulk user creation.

This lab is designed to provide you with a solid foundation in Active Directory management and Windows Server administration. By the end of this tutorial, you'll have a functioning Active Directory environment that mimics a small business network, giving you valuable hands-on experience that can be applied in real-world scenarios.

Let's begin our journey into the world of Active Directory!

<img src="Setup a Home Lab AD Pics folder/Active Directory Powershell lab.drawio.png">

# Section 1: Download Oracle VirtualBox, Windows 10 ISO, and Server 19 ISO

- Use the link to download Oracle VB [Downloads – Oracle VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- Select the platform you are using under VirtualBox Platform Packages and download the VirtualBox Extension Pack
    

- Use the link to download the Windows 10 ISO [Download Windows 10 (microsoft.com)](https://www.microsoft.com/en-us/software-download/windows10)
- Use the link to download the Windows Server 2019 ISO [Windows Server 2019 | Microsoft Evaluation Center](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019)

# Section 2: Setup and Install Server 19 VM

- Start-up Oracle VM VirtualBox Manager
- Click on New
- Name the VM ‘DC’ for domain controller
- Pick ‘Other Windows (64-bit)’ for the Version
- Click Continue
- For Memory size ‘2048 MB’ is fine if you have 8 MB or more of RAM. If you less than 8 MB you will want to lower on the Memory size.
- Start up the Virtual Machine and the VM should start installing everything. This part may take a while depending on your system.
- If a Windows Setup window pops up select the language, time, and keyboard input to your choosing.
- Select Install
- For the operating system select ‘Windows Server 2019 Standard Evaluation (Desktop Experience)’.
- Hit next until you reach ‘Which type of installation do you want?’. Select ‘Custom: Install Windows only (advanced)’.
- Drive 0 should be selected if so, hit next.
- During the installation the VM should restart. If a command line pops up on the VM DO NOT press any keys.
- After a while you should see Customize settings. Enter any password you would like but you will need to remember it. Using password1 is ok since we don’t need to worry about threat actors in a sandbox environment.
- Click finish and log in to the home screen.
- Click on devices and click insert guest additions CD image for a better experience with the VM.
- Click on file explorer in the VM.
- Click on ‘This PC’ and click on ‘CD Drive (D:) VirtualBox Guest Additions’.
- Click on ‘VBoxWindowsAdditions-amd64’.
- A window will pop up and you can just click next on everything and hit Install.
- After the install click on, I want to manually reboot later and hit Finish.
- Click on the windows icon, click the power icon, and click on ‘Shut down’ to avoid any errors.
- Load up the VM again.

# Section 3: Setup IP addressing for Internal NIC

- Click on the network icon on the bottom right of your screen.
- Click on network and then click on ‘Change adapter options.
- Right click on the first Ethernet, click on Status and then details.
- In this window look for IPv4 Address.
- The IP address should have your home IP address listed.
- If so, right click on Ethernet again and click on Rename.
- Rename it to _INTERNET_.
- Right click on Ethernet 2 and rename it to _INTERNAL_.
- This ethernet is trying to look for a DHCP server but is unsuccessful.
- Right click on _INTERNAL_ and click on Properties.
- Click on the text box of ‘Internet Protocol Version 4 (TCP/IPv4)’.
- Edit the IP address to 172.16.0.1, the Subnet mask to 255.255.255.0, the Default gateway will be empty, and the Preferred DNS server is 127.0.0.1.
- Click OK for the two windows.
- Close all the windows and right click the windows start menu in the bottom left of your screen
- Select Systems and click Rename this PC.
- Rename it to ‘DC’ for Domain Controller and hit Next, Restart Now, and Continue.

# Section 4: Installing Active Directory Domain Services and Creating a Domain

- Log back in and in the Server Manager Dashboard click on ‘Add roles and features’.
- Click Next until you reach ‘Server Selection’.
- Click the DC server and press Next.
- In ‘Server Roles’ click on the check box for ‘Active Directory Domain Services’.
- Click Add Features and click Next and click Install when you reach ‘Results’.
- This may take long but once the installation is completed click Close.
- In the top right of Server Manager click on the flag icon with the caution sign.
- Click on ‘Promote this server to a domain controller’.
- Select the Radio button for ‘Add a new forest’ and change the Root domain name to mydomain.com.
- Click next and for the Password it can just be password1.
- Click next until you reached ‘Prerequisites Check’ and click on Install.
- After your VM resets you should see a new account called MYDOMAIN\Administrator account which you can log into.
- On the bottom left of your screen click on the Windows Start icon.
- Click on the ‘Windows Administrative Tools’ and click on the ‘Active Directory Users and Computers’.
- Right click on [mydoamin.com](http://mydoamin.com) and click on New and click on Organizational Unit.
- Name it ‘_ADMINS’ and uncheck the box for ‘Protect container from accidental detection’ and click OK.
- Click on the arrow key next to [mydomain.com](http://mydomain.com) and you should see _ADMINS.
- Right click on _ADMINS and click on New and click on User.
- For the first and last name it can be your real name. So for me my first name is Joshua and my last name is Guzman.
- For the User logon name it is going to be a for admin, space, the first initial of our first name combine with our last name. So ‘a jguzman’ for me.
- Click Next and for the Password it just be password1 again.
- Uncheck the box for ‘User must change password at next logon’.
- Check the box for ‘Password never expires’ and click Next and Finish.
- Now you should see the account we just created under _ADMINS.
- Right click on the user we created and click on Properties.
- Click on ‘Member Of’ and click on Add.
- On the text box type in ‘domain admins’ and click on Check Names and click on OK.
- Click on Apply and OK.
- Go to the windows start icon and sign out.
- Click on Other user and you should see ‘Sign in to: MYDOMAIN’ under Password.
- The username will be the same as we created before. So, a-jguzman for me and password will be password1.

# Section 5: Installing Router Access Service and Network Address Translator

- In the Server Manager click on ‘Add roles and features’.
- Click Next until you reach ‘Server Roles’.
- Click on the box for ‘Remote Access’ and click Next.
- Click on the box for Routing and click Add Features and click Next.
- Click next until you reach Confirmation and click Install.
- This will take a while but once completed click Close.
- On the top right of Server Manager click on Tools and click on ‘Routing and Remote Access’.
- Right click on DC (local) and click on ‘Configure and Enable Routing and Remote Access’.
- Click next and select the radio button for ‘Network address translation (NAT)’.
- Click next and select the radio button for ‘Use this public interface to connect to the internet:’
- If this does not appear just press Cancel close the ‘Routing and Remote Access’ window and reopen ‘Routing and Remote Access’ and ‘Configure and Enable Routing and Remote Access’. This is a common bug.
- Once you are able to select the radio button, click on the _INTERNET_ network interface and click Next.
- Click Finish.
- Close the ‘Routing and Remote Access’ window.

# Section 6: Setup DHCP Server on the Domain Controller

- In Server Manager click on ‘Add roles and features’.
- Click next until you reached ‘Server Roles’ and click on the box for ‘DHCP Server’.
- Click on Add Features and click on next until you reach Confirmation.
- Click Install and once completed click Close.
- On the top right of Server Manager click on Tools and click on DHCP.
- Click on the arrow key of ‘dc.mydomain.com’.
- Right click on IPv4 and click on New Scene.
- Click Next and for the name we can just put the IP range. So, input ‘172.16.0.100-200’ for the name.
- Click Next and the Start IP address will be 172.16.0.100, the End IP address will be 172.16.0.200, and the Length will be 24.
- Click Next until you reach the ‘Router (Default Gateway)’ section.
- For the IP address enter 172.16.0.1 and click add.
- Click Next until you see the Finish button and click on Finish.
- Right click on ‘dc.mydomain.com’ and click on Authorize. Right click on dc again and click on Refresh.

# Section 7: Use PowerShell to Add 1k Users

[AD_PS-master.zip](AD_PS-master.zip)

- In the VM open up internet explorer and use this link to download the file to use in PowerShell
- To get a detail explanation of the Script you can go to my other github link.
- Unzip the file and open the file.
- Open the text file names.txt and enter you first and last name on top of the text document.
- Save and close the text file.
- Click on the windows start icon and click on Windows PowerShell
- Right click on Windows PowerShell ISE, click on more and click on ‘Run as administrator’.
- Click Yes and on top of the PowerShell window click on the open file icon which is called ‘Open Script’.
- Navigate the file explorer to where you downloaded the unzip file and click on ‘1_CREATE_USERS’.
- In the PowerShell terminal type in ‘Set-ExecutionPolicy Unrestricted’, press Enter and click ‘Yes to All’.
- In the PowerShell terminal type in ‘cd C:\users\a-jguzman\desktop\AD_PS-master’ and press Enter.
- On the top of the PowerShell window click on the green play button.
- Click on ‘Run once’ and let it create all the users. If you get errors don’t worry about it since its just duplicate names in the text document.
- It may take a while but once finish open up ‘Active Directory Users and Computers’.
- Right click on [mydomain.com](http://mydomain.com) and click refresh. Click on _USERS and you will see all the users that was just created.
- To find the users by name you can right click on [mydomain.com](http://mydomain.com), click on Find, you can type in the names on Name and click on ‘Find Now’.

# Section 8: Create and Setup Windows 10 VM

- You can now minimize the DC VM and open Oracle VM VirtualBox Manager.
- Click on New, name it CLIENT1, and pick Windows 10 (64-bit) for Version.
- At least 2048 MB of Ram and keep on pressing continue until the CLIENT1 VM is created.
- Right click on CLIENT1 and press on Settings.
- Click on Advanced, on the Shared Clipboard and Drag’n’Drop, change those to Bidirectional.
- Click on Network and for the ‘Attached to’ change it to ‘Internal Network’.
- Run the CLIENT1 VM.
- Click Next and Install Now.
- Click on ‘I don’t have a product key’ and select ‘Windows 10 Pro’ and click Next.
- Click on the check box for ‘I accept the license terms’ and click Next.
- Click on ‘Custom: Install Windows only (advanced)’ and click Next.
- Wait for the install and once you are in the user configurations click Yes, Yes, and skip.
- When you reach to the ‘Let’s connect you to a network’ page click on the ‘I don’t have internet’ on the bottom left of the page.
- Click on ‘Continue with limited setup’.
- In the ‘Who’s going to use this PC?’ page you can enter the name user and click Next.
- Do not enter a password, just click Next.
- In the ‘Choose privacy settings for your device’ page turn all of the options off and click Accept once finished.
- Click on ‘Not Now’ for the Cortana page.
- Once the installation is finish open up Command Prompt
- To test the connection type in ‘ping www.google.com’ and press enter. If you are able to ping to the internet it has worked.

# Section 9: Connect Windows 10 VM to the Domain Controller

- Right click the Windows start icon and click on System.
- Scroll down on the right side of the Settings window until you see ‘Rename this PC (advanced)’ and click on it.
- Click on the Change button and rename it to CLIENT1.
- Click on the ‘Domain:’ radio button and type in ‘mydomain.com’ and click OK.
- The username will be the admin account we created, so for me it’s ‘jguzman’ and the password is password1. After that click OK to everything and click Close on the ‘System Properties’ window.
- Click on Restart Now once the window pops up.
- Open up the DC VM and open up the DHCP window.
- Click on ‘Address Leases’ which is under ‘dc.mydomian.com’ > ‘IPv4’ > ‘Scope [172.16.0.0] 172.16.0.100-200’.
- You can see the CLENT1 VM we just made is now connected to the Domain Controller and the DHCP server gave it an address.

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
