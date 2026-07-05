# CLASSIC FILE SHARE IN AZURE

## Objective

The objective of this task is to create an Azure File Share connect it to an Ubuntu Virtual Machine. Upload files using GUI and CLI. Verify successful file synchronization between Azure Storage and the Ubuntu VM.

## Prerequisites

- Azure Subscription
- Ubuntu Virtual Machine running in Azure
- SSH access to Ubuntu VM
- Storage Account Contributor permissions

## Architecture

Resource Group -> Azure Storage Account -> Azure File Share (SMB) -> Ubuntu Azure VM -> Upload via GUI & CLI -> Access uploaded files

## Step 1: Create a Resource Group and Storage Account

- Created resource group with name `Azure-Lab` under `Azure Subscription 1`

![preview](RG%20creation.png)

- An Azure storage account name must be globally unique across all of Azure.

- Storage account creation with name `requiredstorage` under resource group `Azure-Lab`

![preview](SA%20creation.png)
- Storage account is created

![preview](SA%20created.png)

## Step 2: Create a new Classic File Share 

- Under storage account select `Data Storage -> Classic file shares`

- New classic file share creation with name `sharedfiles`

![preview](Create%20file%20share.png)

-  File share created under storage account `requiredstorage`

![preview](file%20share%20created.png)

## Step 3: Create a Virtual Machine with ubuntu

- VM creation with name `temp-vm` under same resource group `Azure-Lab`

![preview](Create%20VM.png)

- VM is created

![preview](VM%20created.png)

- VM config

![preview](VM%20Config.png)

## Step 4: Uploading files through GUI verifying in CLI

- Connecting VM to terminal through SSH

![preview](SSH%20login.png)

- Run update command 

```bash
sudo apt update -y
``` 
- File with name COBM.jpg uploaded through GUI in classic file share

![preview](Upload%20file%20in%20file%20share.png)

- Once file is uploaded connect it to VM
- Select `Connect -> Linux -> Show Script`

![preview](Connecting%20file%20share%20to%20VM.png)

- Copy the script and run it

```bash
sudo mkdir -p /media/sharedfiles
if [ ! -d "/etc/smbcredentials" ]; then
sudo mkdir /etc/smbcredentials
fi
if [ ! -f "/etc/smbcredentials/requiredstorage.cred" ]; then
    sudo bash -c 'echo "username=requiredstorage" >> /etc/smbcredentials/requiredstorage.cred'
    sudo bash -c 'echo "password=pgrcPFn3ShE0X8RfPncRFgG+XCXdF6IYrK4Ty5Xny5WGaPxWbJif2vjYObJvXSJL+eRqzLb1a4nc+AStRbZSGQ==" >> /etc/smbcredentials/requiredstorage.cred'
fi
sudo chmod 600 /etc/smbcredentials/requiredstorage.cred

sudo bash -c 'echo "//requiredstorage.file.core.windows.net/sharedfiles /media/sharedfiles cifs nofail,credentials=/etc/smbcredentials/requiredstorage.cred,dir_mode=0755,file_mode=0755,serverino,nosharesock,mfsymlinks,actimeo=30" >> /etc/fstab'
sudo mount -t cifs //requiredstorage.file.core.windows.net/sharedfiles /media/sharedfiles -o credentials=/etc/smbcredentials/requiredstorage.cred,dir_mode=0755,file_mode=0755,serverino,nosharesock,mfsymlinks,actimeo=30
```
- checking uploaded files under directory `sharedfiles`

```bash
cd /media/sharedfiles/
ls
```
![preview](Can%20see%20GUI%20uploaded%20data%20in%20CLI.png)

- File is uploaded  

## Step 5: Uploading files through CLI verifying on GUI

- Created a file named `sample.txt` with content.

```bash
echo "Azure File Share Test" > sample.txt
```
- Moving the created file to File Share directory
```bash
sudo mv sample.txt /media/sharedfiles/
```
- Verify if the file is moved to destination
```bash
ls /media/sharedfiles/
```
- Verify if the file can be seen through GUI

![preview](file%20uploaded%20from%20CLI.png)

- File is uploaded and can be seen through GUI

![preview](file%20verified%20in%20GUI.png)


## Output

The Azure Storage Account and Azure File Share were successfully created. The File Share was mounted on an Ubuntu Virtual Machine. File uploads were successfully performed through both the Azure Portal (GUI) and the Ubuntu Command Line Interface (CLI). The successful synchronization of files between Azure Storage and the Ubuntu VM verified proper connectivity and functionality of the Azure File Share.

