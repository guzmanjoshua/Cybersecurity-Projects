# Digitial Forensics

### Introduction

Digital forensics is a branch of forensic science that involves the recovery and investigation of material found in digital devices, often in relation to computer crime. This field is used to gather and preserve evidence from a particular computing device in a way that is suitable for presentation in a court of law. The goal of digital forensics is to explain the exact nature of a digital incident, such as a computer break-in, for example.

Digital forensics CTF (Capture The Flag) is a type of cybersecurity competition where participants solve various digital forensics related challenges. These challenges often involve the use of digital forensics techniques to uncover hidden or deleted information in digital devices or network systems. The goal is to "capture the flag," which is typically a specific piece of information or a solution to a specific problem. These competitions are excellent ways for aspiring digital forensics professionals to practice and hone their skills.

Autopsy is a digital forensics platform and graphical interface to The Sleuth Kit and other digital forensics tools. It is used by law enforcement, military, and corporate examiners to investigate what happened on a computer. The platform is open-source and it allows investigators to locate deleted files, review file activity timelines, and perform keyword searches, among other digital investigative tasks. It is a very useful tool in the field of digital forensics.

## Section 1: Download Autopsy Digitial Forensics and CFReDS Portal File

Download Autopsy & CFReDS File: **STEP 1**

- Download Autopsy on their official website: [Autopsy - Digital Forensics](https://www.autopsy.com/)
- Go to the CFReDS Portal website: [CFReDS Portal (nist.gov)](https://cfreds.nist.gov/)
- For this lab choose ‘Forensics Image Test image’.

<img src="Digitial Forensics Pics Folder/ADF 1.png">

Download Autopsy & CFReDS File: **STEP 2**

- Select ‘drive.google.com/file/d/1Fd1pX1r4waRkD6Z2O8J5cRZyeSNU5-SY/view?usp=sharing’.
- Download the file that pops up.

<img src="Digitial Forensics Pics Folder/ADF 2.png">

 

## Section 2: Open Autopsy and Create a Case

Create a Case: **STEP 1**

- Open Autopsy once it is downloaded.
- Click on New Case.

<img src="Digitial Forensics Pics Folder/ADF 3.png">

Create a Case: **STEP 2**

- Name the case in Case Name.
- Press Next.

<img src="Digitial Forensics Pics Folder/ADF 4.png">

Create a Case: **STEP 3**

- Skip the information here and press Finish.

<img src="Digitial Forensics Pics Folder/ADF 5.png">

## Section 3: Add Data Source

Add Data Source: **STEP 1**

- On Select Host press Next

<img src="Digitial Forensics Pics Folder/ADF 6.png">

Add Data Source: **STEP 2**

- Double Check to see if only the ‘Disk Image or VM File’ is enable.
- Press Next.

<img src="Digitial Forensics Pics Folder/ADF 7.png">

Add Data Source: **STEP 3**

- On File Explorer, go where you downloaded the ‘2020JimmyWilson.E01’ file, which was downloaded from the ‘drive.google.com/file/d/1Fd1pX1r4waRkD6Z2O8J5cRZyeSNU5-SY/view?usp=sharing’ URL from the beginning steps.
- Right click on the file.
- Left click on Copy as path.

<img src="Digitial Forensics Pics Folder/ADF 8.png">

Add Data Source: **STEP 4**

- Back to Autopsy, paste the path on the Path text box.
- Change the time zone to your time zone.
- Press Next.

<img src="Digitial Forensics Pics Folder/ADF 9.png">

Add Data Source: **STEP 5**

- Leave the select boxes as is.
- Press Next.

<img src="Digitial Forensics Pics Folder/ADF 10.png">

Add Data Source: **STEP 6**

- Select Finish.

<img src="Digitial Forensics Pics Folder/ADF 11.png">

## Section 4: Explore The Data

Now that the data has been added, we can start exploring it. While you can explore the data freely, I will highlight the crucial information and guide you on how to navigate to it in the following steps.

Explore The Data: **STEP 1**

- To view the images, click the File Views dropdown button.
- Click the File Types dropdown button.
- Click the By Extension dropdown button.
- Click the Images button.
- Click the Thumbnail tab on the Listing tab.

As you scroll through the images, the most noticeable ones are those of the driver's license printer and the licensing insurance systems.

<img src="Digitial Forensics Pics Folder/ADF 12.png">

Explore The Data: **STEP 2**

- To view the plain text, click the Documents dropdown button.
- Click the Plain Text button.
- Click the Text tab on the Data Content tab.

As you scroll through the plain text, the most noticeable text is the new price list. This list appears to include prices for credit cards, ID cards, green cards, driver’s licenses, and insurance certificates to print and send.

<img src="Digitial Forensics Pics Folder/ADF 13.png">

Explore The Data: **STEP 3**

- To view the emails, click the Data Artifacts dropdown button.
- Click the E-Mail Messages dropdown button.
- Click the Default ([Default]) dropdown button.
- Click the Default button.
- Click an eml file on the Listing tab.
- Click on Data Artifacts tab on the Data Content tab.
- Click on HTML tab on the Data Artifacts tab.
- Some eml files may have attachments. If so, click on the Attachments tab on the Data Artifacts tab.
- Click on the attachment file you want to open.
- Click on the View in New Window button, to view the file.
- If the attachment file(s) are pictures, you can click on the Thumbnail tab on the Attachments tab to easily view the pictures.

As you scroll through the emails, the ones that stand out are those where the new price list text file, which we saw earlier, was sent. We can see the emails and names it was from and sent to.

<img src="Digitial Forensics Pics Folder/ADF 14.png">

## Section 5: Piece it Together

With the information we gather we can conclude that the man that owns this data is name Jimmy Wilson base on the name of the data sources and emails that was sent and received. Jimmy is committing identity fraud since he prints out IDs, licenses, and certificates. We also discover Jimmy is in a collusion with two other people with the of first names of Robert and Jose based on emails. 

While exploring a portion of Jimmy's data, we gathered some information about him. However, there is still more to discover. In this lab, we provided a starting point for using digital forensics and exploring data. Use this knowledge to investigate Jimmy further, such as reviewing his web history, installed programs, recycle bin, and other sources.

### Conclusion

In conclusion, digital forensics plays a fundamental role in investigating and piecing together information from digital devices. It is a powerful tool in uncovering hidden or deleted information. Through this lab, we have gained an understanding of how to use Autopsy, a digital forensics platform, to explore various types of data including images, texts, and emails. We have also seen how this information can be used to form a profile of an individual. While we have only scratched the surface of this vast field, this knowledge provides a solid foundation for further exploration into the realm of digital forensics.
