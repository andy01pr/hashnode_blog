# How to use Azure Files as a backup folder

Using [**Azure Files**](https://docs.microsoft.com/en-us/azure/storage/files/storage-files-introduction) is a great way to back up your files as it was just another folder on your computer. This is similar to other services like Dropbox or Google Drive where you have online-only files as an option.

To do this create an account in Microsoft Azure if you don't have one, then go to the Azure Portal. Once there go to the Storage Accounts section and create a new Azure Storage.

The following image shows the minimum information needed to create a Storage Account. In this case, I set the **Performance** option as **"Standard"** as I do not need the more expensive options. I set the **"Locally-redundant storage (LRS)"** for **redundancy**, but you can choose other types of redundancy needed for your situation.

![](https://thecodingtips.com/content/images/2022/08/Creating_Storage.png align="left")

Once the Storage Account is created, go to the **"File Shares"** section.

![](https://thecodingtips.com/content/images/2022/08/Newly_Created_Storage-2.png align="left")

Once in the **"File Shares"** section create a new File Share, in this window you have the option to select the **Tier (Transaction Optimized, Hot and Cool)**, I selected the **Hot Tier** because is the [most cost-effective](https://azure.microsoft.com/en-us/pricing/details/storage/files/) of the options.

![](https://thecodingtips.com/content/images/2022/08/Creating_File_Share.png align="left")

Once the File Share is created, press the **"Connect"** option and a side window will appear with the **PowerShell** script to run and create a Network connection to the File Share.

![](https://thecodingtips.com/content/images/2022/08/Connecting_To_FIle_Share-1.png align="left")

![](https://thecodingtips.com/content/images/2022/08/Powershell_Script.png align="left")

Copy the script and open PowerShell on your computer.

![](https://thecodingtips.com/content/images/2022/08/Powershell_Terminal.png align="left")

Paste the code in the terminal and it will run automatically. There will be a pause when the script is running, just press the Enter key and it will continue.

![](https://thecodingtips.com/content/images/2022/08/Running_Powershell_Script.png align="left")

After running the script you will see in **"My Computer"** in the Network Locations section the connection to the File Share.

![](https://thecodingtips.com/content/images/2022/08/My_Computer_Share.png align="left")

And that's it, you can now copy and paste any file to Azure File Share using this connection.

**One important note, this only works from Windows Server 2012 and Windows 8 or later. It could work in Windows 7 but without encryption which is not recommended at all. For that, you would have to disable the "Secure transfer required" option in the Azure Storage Configuration.**

![](https://thecodingtips.com/content/images/2022/08/Secure_Transfer_Option.png align="left")

## **Using The Azure File Share as backup**

I've created a sample folder with several files which I want to backup to the Azure File Share. I can manually copy and paste the files, but if I want to automate this I can use Robocopy to transfer the files.

![](https://thecodingtips.com/content/images/2022/08/Desktop_TestFolder.png align="left")

Notice in the image below that the destination folder is the drive letter **z:** which is mapped to the Azure File Share. This will copy all the files in the "TestFolder" to the root folder of the share, and adjust to your needs.

![](https://thecodingtips.com/content/images/2022/08/Robocopy_Copying.png align="left")

![](https://thecodingtips.com/content/images/2022/08/Robocopy_Finished.png align="left")

Once the process of copying files is done, check the Azure File Share in My Computer and the Azure Portal and you will see the uploaded files.

![](https://thecodingtips.com/content/images/2022/08/File_Share_Copied_Files.png align="left")

## **Conclusion**

Azure File Share is an easy way to store files or entire directories depending on your situation. If your application requires storing files in a folder, this is a way to store them safely in the cloud without extensive modifications to the existing infrastructure.

[What is Azure Files?](https://docs.microsoft.com/en-us/azure/storage/files/storage-files-introduction)

[Azure Files pricing](https://azure.microsoft.com/en-us/pricing/details/storage/files/)