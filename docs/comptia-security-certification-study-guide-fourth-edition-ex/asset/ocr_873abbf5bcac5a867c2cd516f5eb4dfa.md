# EXERCISE 21-4

# Using Autopsy to Investigate the Local Disk

In this exercise, you use Autopsy to investigate the contents of your local hard drive and locate items such as Internet sites visited and user accounts as well as to identify encrypted files on the system.

+   1. Download Autopsy from https://www.autopsy.com/download/ and install it on your system.
+   2. After installing Autopsy, right-click the Autopsy icon on the Desktop and choose Run As Administrator. Choose Yes on the User Account Control dialog box about elevating your privileges.
+   3. Choose New Case, enter the following information, and then choose Next:
+      - 20210603_Exer21-4
+      - Leave all other options as is.
+   4. Type the following optional information for the case and then choose Finish:
+      - Number: 20210603_Exer21-4
+      - Examiner Name: Bob Smith
+      - Phone: 555-5555
+      - Email: bobsmith@companyxyz.loc
+   5. When asked to add a data source, note that Local Disk is selected. Also note that you can add a disk image (which you created in the previous exercises). In this exercise, we are going to add your local disk because it will allow you to see information about your own system. Choose Next.
+   6. Choose Select Disk and then choose the hard drive that Windows is installed on (in my case it is Drive 0 of 447GB). Choose OK.
+   7. Choose Next.
+   8. Autopsy wants to use ingest modules on the drive. These are used to located key information on the drive, such as web sites visited. Leave all the modules selected and choose Next. Note that it will take some time to add the drive because it is parsing the information due to the ingest modules.
+   9. After the local drive has been added to Autopsy, expand out Results on the left and then expand Extracted Content. This is where Autopsy displays information typically useful to an investigator about the contents of the drive.