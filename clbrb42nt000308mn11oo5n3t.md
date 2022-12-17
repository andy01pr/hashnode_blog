# How to use Azure Files as a backup folder

Using [**Azure Files**](https://docs.microsoft.com/en-us/azure/storage/files/storage-files-introduction) is a great way to back up your files as it was just another folder on your computer. This is similar to other services like Dropbox or Google Drive where you have online-only files as an option.

To do this create an account in Microsoft Azure if you don't have one, then go to the Azure Portal. Once there go to the Storage Accounts section and create a new Azure Storage.

The following image shows the minimum information needed to create a Storage Account. In this case, I set the **Performance** option as **"Standard"** as I do not need the more expensive options. I set the **"Locally-redundant storage (LRS)"** for **redundancy**, but you can choose other types of redundancy needed for your situation.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671244201409/eZNBz0J0s.png align="center")

Once the Storage Account is created, go to the **"File Shares"** section.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671244445076/8VLTh7Chf.png align="center")

Once in the **"File Shares"** section create a new File Share, in this window you have the option to select the **Tier (Transaction Optimized, Hot and Cool)**, I selected the **Hot Tier** because is the [most cost-effective](https://azure.microsoft.com/en-us/pricing/details/storage/files/) of the options.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671244267929/X2aLvERll.png align="center")

Once the File Share is created, press the **"Connect"** option and a side window will appear with the **PowerShell** script to run and create a Network connection to the File Share.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671244285248/aHxVMox1J.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671244310289/WLc5YtfSE.png align="center")

Copy the script and open PowerShell on your computer.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671244331367/BK5qSCm0W.png align="center")

Paste the code in the terminal and it will run automatically. There will be a pause when the script is running, just press the Enter key and it will continue.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671244356940/5KsOCEgMU.png align="center")

After running the script you will see in **"My Computer"** in the Network Locations section the connection to the File Share.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671244370385/CpWq2uToJ.png align="center")

And that's it, you can now copy and paste any file to Azure File Share using this connection.

**One important note, this only works from Windows Server 2012 and Windows 8 or later. It could work in Windows 7 but without encryption which is not recommended at all. For that, you would have to disable the "Secure transfer required" option in the Azure Storage Configuration.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671244382344/RMy8sfXuB.png align="center")

## **Using The Azure File Share as backup**

I've created a sample folder with several files which I want to backup to the Azure File Share. I can manually copy and paste the files, but if I want to automate this I can use Robocopy to transfer the files.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671244395516/gAcD0Dj8W.png align="center")

Notice in the image below that the destination folder is the drive letter **z:** which is mapped to the Azure File Share. This will copy all the files in the "TestFolder" to the root folder of the share, and adjust to your needs.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671244405882/xwXsBslKb.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671244417172/a2ETxPlGP.png align="center")

Once the process of copying files is done, check the Azure File Share in My Computer and the Azure Portal and you will see the uploaded files.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671244427695/x3K03H4kr.png align="center")

## **Conclusion**

Azure File Share is an easy way to store files or entire directories depending on your situation. If your application requires storing files in a folder, this is a way to store them safely in the cloud without extensive modifications to the existing infrastructure.

[What is Azure Files?](https://docs.microsoft.com/en-us/azure/storage/files/storage-files-introduction)

[Azure Files pricing](https://azure.microsoft.com/en-us/pricing/details/storage/files/)