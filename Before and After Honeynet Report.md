# Before and After Honeynet Report

I made an Azure honeynet by turning off all protections locally for two VMs, a Windows 10 Pro Machine with an MS SQL Database, and an Ubuntu 20.04 Linux Server. To gather the most data, I added a Network Security Group Inbound Rule to allow connections from any IP & Port to Any IP & Port, and then 24 hours later, I removed that rule and turned on NIST 800-53 Rev.5 with extra Boundary Protections from the System and Communications Protection family to secured it. The picture below illustrates the improvements in security after 24 hours.

To learn more on how to setup an Azure Honeynet into a NIST Compliant SOC with Microsoft Sentinel and Custom Workbooks, click the link on the right: [Joshua Guzman - Azure Honeynet into a NIST Compliant SOC GitHub Project](https://github.com/guzmanjoshua/Cybersecurity-Projects/blob/main/Turning%20an%20Azure%20Honeynet%20into%20a%20NIST%20Compliant%20SOC.md)

![HN 51.png](Before%20and%20After%20Honeynet%20Report%20b4b8246a108e4fb4ad6dbaf414bed6f4/HN_51.png)