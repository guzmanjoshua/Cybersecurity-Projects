# Explanation of the 1000 Users Script (1_CREATE_USERS)

![image.png](image.png)

Line 2 sets all the Users passwords as ‘Password1’.
Line 3 collects all the names in the names.txt file and put them in an array.

Line 6 converts the string into a SecureString or a password object.
Line 7 creates a new file called _USERS under [mydomain.com](http://mydomain.com) in Active Directory Users and Computers. The ‘-ProtectedFromAccidentalDeletion $False’ portion unchecks the named box.

![image.png](image%201.png)

![image.png](image%202.png)

Line 9 calls for each object in the $USER_FIRST_LAST_LIST array created in line 3.
Line 10 will take the first name as a string by detecting and splitting the first name by the first blank space it detects. Calling [0] will only check the first word in the string.
Line 11 will take the last name as a string by detecting and splitting the last name by the first blank space it detects. Calling [1] will only check the second word in the string.

Line 12 will take the first letter from the first name and attached it in from of the last name and create it as a string called $username.
Line 13 will just output in Windows PowerShell what user is being created

Line 15-23 creates a new user in active directory using the variables that were created and using the appropriate variable. Line 22 will put the user into the _USERS file that was created in line 7.