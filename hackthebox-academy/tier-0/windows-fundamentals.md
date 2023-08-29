---
cover: https://academy.hackthebox.com/storage/modules/49/logo.png
coverY: 0
layout:
  cover:
    visible: true
    size: hero
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Windows Fundamentals

## Versions

All Windows versions :

| OS Name                              | Version |
| ------------------------------------ | ------- |
| Windows NT 4                         | 4.0     |
| Windows 2000                         | 5.0     |
| Windows XP                           | 5.1     |
| Windows Server 2003, 2003 R2         | 5.2     |
| Windows Vista, Server 2008           | 6.0     |
| Windows 7, Server 2008 R2            | 6.1     |
| Windows 8, Server 2012               | 6.2     |
| Windows 8.1, Server 2012 R2          | 6.3     |
| Windows 10, Server 2016, Server 2019 | 10.0    |

Show the Windows Version and BuildNumber :

```
Get-WmiObject -Class Win32_OperatingSystem | select Version, BuildNumber
Get-WmiObject -Class Win32_Service
Get-WmiObject -Class Win32_Bios
```

`Get-WmiObject` can be used to start and stop services.

Examples of remote access :

* Virtual Private Networks (VPN)
* Secure Shell (SSH)
* File Transfer Protocol (FTP)
* Virtual Network Computing (VNC)
* Windows Remote Management (or PowerShell Remoting) (WinRM)
* Remote Desktop Protocol (RDP)

## RDP

RDP means **Remote Desktop Protocol.**

Types of connection :

* Windows (C) - Windows (S), use built-in application `mstsc.exe`.
* Linux (C) - Windows (S), use `xfreerdp`, `remmina` or `rdesktop`.

The remote access on Windows is not allowed by default. It exists interresting saved remote desktop files `.rdp`.

```bash
xfreerdp /v:10.129.201.57 /u:htb-student /p:Academy_WinFun!
```

## Structure

<table><thead><tr><th width="199">Directory</th><th>Function</th></tr></thead><tbody><tr><td>Perflogs</td><td>Can hold Windows performance logs but is empty by default.</td></tr><tr><td>Program Files</td><td>On 32-bit systems, all 16-bit and 32-bit programs are installed here. On 64-bit systems, only 64-bit programs are installed here.</td></tr><tr><td>Program Files (x86)</td><td>32-bit and 16-bit programs are installed here on 64-bit editions of Windows.</td></tr><tr><td>ProgramData</td><td>This is a hidden folder that contains data that is essential for certain installed programs to run. This data is accessible by the program no matter what user is running it.</td></tr><tr><td>Users</td><td>This folder contains user profiles for each user that logs onto the system and contains the two folders Public and Default.</td></tr><tr><td>Default</td><td>This is the default user profile template for all created users. Whenever a new user is added to the system, their profile is based on the Default profile.</td></tr><tr><td>Public</td><td>This folder is intended for computer users to share files and is accessible to all users by default. This folder is shared over the network by default but requires a valid network account to access.</td></tr><tr><td>AppData</td><td>Per user application data and settings are stored in a hidden user subfolder (i.e., cliff.moore\AppData). Each of these folders contains three subfolders. The Roaming folder contains machine-independent data that should follow the user's profile, such as custom dictionaries. The Local folder is specific to the computer itself and is never synchronized across the network. LocalLow is similar to the Local folder, but it has a lower data integrity level. Therefore it can be used, for example, by a web browser set to protected or safe mode.</td></tr><tr><td>Windows</td><td>The majority of the files required for the Windows operating system are contained here.</td></tr><tr><td>System, System32, SysWOW64</td><td>Contains all DLLs required for the core features of Windows and the Windows API. The operating system searches these folders any time a program asks to load a DLL without specifying an absolute path.</td></tr><tr><td>WinSxS</td><td>The Windows Component Store contains a copy of all Windows components, updates, and service packs.</td></tr></tbody></table>

```bash
dir c:\ /a
tree "c:\Program Files (x86)\VMware"
tree c:\ /f | more
```

## File System

Types of Windows file systems :

* FAT12
* FAT16
* FAT32
* NTFS

NTFS permissions :

<table data-header-hidden><thead><tr><th width="262"></th><th></th></tr></thead><tbody><tr><td>Permission Type</td><td>Description</td></tr><tr><td>Full Control</td><td>Allows reading, writing, changing, deleting of files/folders.</td></tr><tr><td>Modify</td><td>Allows reading, writing, and deleting of files/folders.</td></tr><tr><td>List Folder Contents</td><td>Allows for viewing and listing folders and subfolders as well as executing files. Folders only inherit this permission.</td></tr><tr><td>Read and Execute</td><td>Allows for viewing and listing files and subfolders as well as executing files. Files and folders inherit this permission.</td></tr><tr><td>Write</td><td>Allows for adding files to folders and subfolders and writing to a file.</td></tr><tr><td>Read</td><td>Allows for viewing and listing of folders and subfolders and viewing a file's contents.</td></tr><tr><td>Traverse Folder</td><td>This allows or denies the ability to move through folders to reach other files or folders. For example, a user may not have permission to list the directory contents or view files in the documents or web apps directory in this example c:\users\bsmith\documents\webapps\backups\backup_02042020.zip but with Traverse Folder permissions applied, they can access the backup archive.</td></tr><tr><td>Full control</td><td>Users are permitted or denied permissions to add, edit, move, delete files &#x26; folders as well as change NTFS permissions that apply to all permitted folders</td></tr><tr><td>Traverse folder / execute file</td><td>Users are permitted or denied permissions to access a subfolder within a directory structure even if the user is denied access to contents at the parent folder level. Users may also be permitted or denied permissions to execute programs</td></tr><tr><td>List folder/read data</td><td>Users are permitted or denied permissions to view files and folders contained in the parent folder. Users can also be permitted to open and view files</td></tr><tr><td>Read attributes</td><td>Users are permitted or denied permissions to view basic attributes of a file or folder. Examples of basic attributes: system, archive, read-only, and hidden</td></tr><tr><td>Read extended attributes</td><td>Users are permitted or denied permissions to view extended attributes of a file or folder. Attributes differ depending on the program</td></tr><tr><td>Create files/write data</td><td>Users are permitted or denied permissions to create files within a folder and make changes to a file</td></tr><tr><td>Create folders/append data</td><td>Users are permitted or denied permissions to create subfolders within a folder. Data can be added to files but pre-existing content cannot be overwritten</td></tr><tr><td>Write attributes</td><td>Users are permitted or denied to change file attributes. This permission does not grant access to creating files or folders</td></tr><tr><td>Write extended attributes</td><td>Users are permitted or denied permissions to change extended attributes on a file or folder. Attributes differ depending on the program</td></tr><tr><td>Delete subfolders and files</td><td>Users are permitted or denied permissions to delete subfolders and files. Parent folders will not be deleted</td></tr><tr><td>Delete</td><td>Users are permitted or denied permissions to delete parent folders, subfolders and files.</td></tr><tr><td>Read permissions</td><td>Users are permitted or denied permissions to read permissions of a folder</td></tr><tr><td>Change permissions</td><td>Users are permitted or denied permissions to change permissions of a file or folder</td></tr><tr><td>Take ownership</td><td>Users are permitted or denied permission to take ownership of a file or folder. The owner of a file has full permissions to change any permissions</td></tr></tbody></table>

The permissions are recursive by default.

The Integrity Control Access Control List ([icacls](https://ss64.com/nt/icacls.html)) is a Windows tool to manage permissions.

```powershell
icacls c:\windows
icacls c:\Users /grant joe:f
icacls c:\Users /remove joe
```

NTFS Inheritance settings :

<table data-header-hidden><thead><tr><th width="118"></th><th></th></tr></thead><tbody><tr><td>Acronym</td><td>Description</td></tr><tr><td>CI</td><td>container inherit</td></tr><tr><td>OI</td><td>object inherit</td></tr><tr><td>IO</td><td>inherit only</td></tr><tr><td>NP</td><td>do not propagate inherit</td></tr><tr><td>I</td><td>permission inherited from parent container</td></tr></tbody></table>

Share permissions != NTFS basic permissions. Share persmissions :

<table data-header-hidden><thead><tr><th width="149"></th><th></th></tr></thead><tbody><tr><td>Permission</td><td>Description</td></tr><tr><td>Full Control</td><td>Users are permitted to perform all actions given by Change and Read permissions as well as change permissions for NTFS files and subfolders</td></tr><tr><td>Change</td><td>Users are permitted to read, edit, delete and add files and subfolders</td></tr><tr><td>Read</td><td>Users are allowed to view file &#x26; subfolder contents</td></tr></tbody></table>

## Create a network shared on Windows

On Windows server :

* Create a folder.
* Go to advanced sharing properties and customize it, you can see :
  * Access Control List (ACL) for shared ressource
  * Access Control Entries (ACE) for users and groups

On Linux Client :

```bash
smbclient -L target -U username
sudo mount -t cifs -o username=htb-student,password=Academy_WinFun! //target/"Company Data" /home/user/Desktop/
```

If troubleshots, check the firewall settings.

On Windows server :

```powershell
net share       # show shared ressources
```

There are 2 other tools to manages shared ressources :

* Computer Management (System Tools > Shared Folders > Shares)
* Event Viewer (Windows Logs > Security)

## Services and processes

Services are a major component of the Windows operating system. They allow for the creation and management of long-running processes. You can manage Windows services with the Service Control Manager (SCM) and execute `services.msc`.

Show services with Posershell :

```powershell
Get-Service | ? {$_.Status -eq "Running"} | select -First 2 | fl
```

Critical system services cannot be stopped and restarted without a restart.

* `smss.exe` : Session Manager SubSystem. Responsible for handling sessions on the system.
* `csrss.exe` : Client Server Runtime Process. The user-mode portion of the Windows subsystem.
* `wininit.exe` : Starts the Wininit file .ini file that lists all of the changes to be made to Windows when the computer is restarted after installing a program.
* `logonui.exe` : Used for facilitating user login into a PC.
* `lsass.exe` : The Local Security Authentication Server verifies the validity of user logons to a PC or server. It generates the process responsible for authenticating users for the Winlogon service.
* `services.exe` : Manages the operation of starting and stopping services.
* `winlogon.exe` : Responsible for handling the secure attention sequence, loading a user profile on logon, and locking the computer when a screensaver is running.
* `system` : A background system process that runs the Windows kernel.
* `svchost.exe` with RPCSS : Manages system services that run from dynamic-link libraries (files with the extension .dll) such as "Automatic Updates," "Windows Firewall," and "Plug and Play." Uses the Remote Procedure Call (RPC) Service (RPCSS).
* `svchost.exe` with Dcom/PnP : Manages system services that run from dynamic-link libraries (files with the extension .dll) such as "Automatic Updates," "Windows Firewall," and "Plug and Play." Uses the Distributed Component Object Model (DCOM) and Plug and Play (PnP) services.

Processes run in the background on Windows systems. They either run automatically as part of the Windows operating system or are started by other installed applications.

List of process tools :

* Local Security Authority Subsystem Service (LSASS)
* Sysinternal Tools or `\\live.sysinternals.com\tools`
* Task Manager `taskmgr`
* Ressource Monitor
* Process Explorer

## Service Permissions

The service is configured to run as the user who installed it. The GUI tool is `service.msc`, you can see permissions, behaviour in case of failure, absolute path, etc. You can check these configurations to check if it exists a weak permission.

Most services run with LocalSytem privileges by default.

Built-in service accounts in Windows :

* LocalService
* NetworkService
* LocalSystem

It is possible to create a specific account only for running services.

Examine services using `sc` :

```powershell
sc qc wuauserv                                                      # Service Controller, Query Command
sc stop wuauserv                                                    # stop service
​
# with elevated privileges
sc config wuauserv binPath=C:\Winbows\PrefectlyLegitProgram.exe     # malware
​
sc sdshow wuauserv
```

It returns

```
D:(A;;CCLCSWRPLORC              ;;;AU)
  (A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;BA)
  (A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;SY)

S:(AU;FA;CCDCLCSWRPWPDTLOSDRCWDWO;;;WD)
```

<table data-header-hidden data-full-width="true"><thead><tr><th width="126.33333333333331"></th><th width="317"></th><th></th></tr></thead><tbody><tr><td>Character</td><td>Full name</td><td>Description</td></tr><tr><td>D</td><td></td><td>the proceeding characters are DACL permissions</td></tr><tr><td>AU</td><td></td><td>defines the security principal Authenticated Users</td></tr><tr><td>A</td><td></td><td>access is allowed</td></tr><tr><td>CC</td><td>SERVICE_QUERY_CONFIG</td><td>it is a query to the service control manager (SCM) for the service configuration</td></tr><tr><td>LC</td><td>SERVICE_QUERY_STATUS</td><td>it is a query to the service control manager (SCM) for the current status of the service</td></tr><tr><td>SW</td><td>SERVICE_ENUMERATE_DEPENDENTS</td><td>it will enumerate a list of dependent services</td></tr><tr><td>RP</td><td>SERVICE_START</td><td>it will start the service</td></tr><tr><td>LO</td><td>SERVICE_INTERROGATE</td><td>it will query the service for its current status</td></tr><tr><td>RC</td><td>READ_CONTROL</td><td>it will query the security descriptor of the service</td></tr></tbody></table>

To show service description with Powershell :

```powershell
Get-ACL -Path HKLM:\System\CurrentControlSet\Services\wuauserv | Format-List
```

## Windows sessions

An interactive session asks credentials and a non-interactive session can be used by non standard user accounts. Types of non-interactive accounts :

<table data-header-hidden><thead><tr><th width="243"></th><th></th></tr></thead><tbody><tr><td>Account</td><td>Description</td></tr><tr><td>Local System Account</td><td>Also known as the <code>NT AUTHORITY\SYSTEM</code> account, this is the most powerful account in Windows systems. It is used for a variety of OS-related tasks, such as starting Windows services. This account is more powerful than accounts in the local administrators group.</td></tr><tr><td>Local Service Account</td><td>Known as the <code>NT AUTHORITY\LocalService</code> account, this is a less privileged version of the SYSTEM account and has similar privileges to a local user account. It is granted limited functionality and can start some services.</td></tr><tr><td>Network Service Account</td><td>This is known as the <code>NT AUTHORITY\NetworkService</code> account and is similar to a standard domain user account. It has similar privileges to the Local Service Account on the local machine. It can establish authenticated sessions for certain network services.</td></tr></tbody></table>

You can login an interactive session with `runas` command.

### Interacting with the Windows Operating System

The full path of `cmd` is `C:\Windows\system32\cmd.exe`. Type `help` to get some help (and specify the command to get command help). 🙂

```
help
help schtasks
ipconfig /?
```

Powershell has cmdlets that follow the form `Verb-Noun` and take flag or arguments.

```
Get-ChildItem
Get-ChildItem -Hidden
Get-ChildItem -Recurse
Get-ChildItem -Path C:\Users\Administrator\Documents
Get-ChildItem -Path C:\Users\Administrator\Documents -Recurse

Set-Location
cd				# alias
ls				# alias

Get-ChildItem
ls				# alias
gci				# alias

Get-Alias
Get-Alias -Name
New-Alias

Help <cmdlet_name> -Online
Get-Help Get-AppPackage

.\PowerView.ps1 ; Get-LocalGroup | fl				# run scripts
Get-Module | select Name, ExportedCommands | fl		# show all available modules
```

It exists a file running protection called `execution policy` :

| Policy       | Description                                                                                                                                                                                                                                                      |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| AllSigned    | All scripts can run, but a trusted publisher must sign scripts and configuration files. This includes both remote and local scripts. We receive a prompt before running scripts signed by publishers that we have not yet listed as either trusted or untrusted. |
| Bypass       | No scripts or configuration files are blocked, and the user receives no warnings or prompts.                                                                                                                                                                     |
| Default      | This sets the default execution policy, Restricted for Windows desktop machines and RemoteSigned for Windows servers.                                                                                                                                            |
| RemoteSigned | Scripts can run but requires a digital signature on scripts that are downloaded from the internet. Digital signatures are not required for scripts that are written locally.                                                                                     |
| Restricted   | This allows individual commands but does not allow scripts to be run. All script file types, including configuration files (.ps1xml), module script files (.psm1), and PowerShell profiles (.ps1) are blocked.                                                   |
| Undefined    | No execution policy is set for the current scope. If the execution policy for ALL scopes is set to undefined, then the default execution policy of Restricted will be used.                                                                                      |
| Unrestricted | This is the default execution policy for non-Windows computers, and it cannot be changed. This policy allows for unsigned scripts to be run but warns the user before running scripts that are not from the local intranet zone.                                 |

```
Get-ExecutionPolicy -List
Set-ExecutionPolicy Bypass -Scope Process
```

### Windows Management Instrumentation (WMI)

WMI components :

| Component Name     | Description                                                                                                                                                                        |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| WMI service        | The Windows Management Instrumentation process, which runs automatically at boot and acts as an intermediary between WMI providers, the WMI repository, and managing applications. |
| Managed objects    | Any logical or physical components that can be managed by WMI.                                                                                                                     |
| WMI providers      | Objects that monitor events/data related to a specific object.                                                                                                                     |
| Classes            | These are used by the WMI providers to pass data to the WMI service.                                                                                                               |
| Methods            | These are attached to classes and allow actions to be performed. For example, methods can be used to start/stop processes on remote machines.                                      |
| WMI repository     | A database that stores all static data related to WMI.                                                                                                                             |
| CMI Object Manager | The system that requests data from WMI providers and returns it to the application requesting it.                                                                                  |
| WMI API            | Enables applications to access the WMI infrastructure.                                                                                                                             |
| WMI Consumer       | Sends queries to objects via the CMI Object Manager.                                                                                                                               |

```
wmic /?
wmic computersystem get name
wmic os list brief				# the os description, with its serial number

Get-WmiObject -Class Win32_OperatingSystem | select SystemDirectory,BuildNumber,SerialNumer,Version | ft
```

### Microsoft Management Console (MMC)

It is used to group snap-ins which are software plug-ins. Type `mmc` on the start menu to open it.

### Windows Subsystem for Linux (WSL)

To install WSL :

```
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsytem-Linux
```

### Security IDentifier (SID)

The system generates for every user a unique SID, they ahve different length and are stored in the security database.

```
whoami /user		# ws01\bob S-1-5-21-674899381-4069889467-2080702030-1002
```

The SID is broken is several parts : `(SID)-(revision level)-(identifier-authority)-(subauthority1)-(subauthority2)-(etc)`

| Number                          | Meaning              | Description                                                                                                                                                                                        |
| ------------------------------- | -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| S                               | SID                  | Identifies the string as a SID.                                                                                                                                                                    |
| 1                               | Revision Level       | To date, this has never changed and has always been 1.                                                                                                                                             |
| 5                               | Identifier-authority | A 48-bit string that identifies the authority (the computer or network) that created the SID.                                                                                                      |
| 21                              | Subauthority1        | This is a variable number that identifies the user's relation or group described by the SID to the authority that created it. It tells us in what order this authority created the user's account. |
| 674899381-4069889467-2080702030 | Subauthority2        | Tells us which computer (or domain) created the number                                                                                                                                             |
| 1002                            | Subauthority3        | The RID that distinguishes one account from another. Tells us whether this user is a normal user, a guest, an administrator, or part of some other group                                           |

### Security Account Manager (SAM)

It grants rights to a network to execute specific processes. The access rights are managed by Access Control Entries (ACE) in Access Control List (ACL). ACLs contain ACEs. ACEs define who can execute a process.

Types of ACL :

* Discretionary Access Control List (DACL)
* System Access Control List (SACL)

### User Account Control (UAC)

\<!-- !\[src]\(../images/uacarchitecture1.png) -->

### Registry

It's a hierarchical database for the OS. It stores low-level settings.

It has root keys like `HKEY_LOCAL_MACHINE` and subkeys like `HARDWARE`. The root key starts with HKEY. The files system registry files are stored in `C:\Windows\System32\Config\`. The user-specific registry hive (HKCU) are stored in `C:\Windows\Users\<username>\Ntuser.dat`.

```
regedit
```

Types of values for a key :

* REG\_BINARY
* REG\_DWORD
* REG\_DWORD\_LITTLE\_ENDIAN
* REG\_DWORD\_BIG\_ENDIAN
* REG\_EXPAND\_SZ
* REG\_LINK
* REG\_MULTI\_SZ
* REG\_NONE
* REG\_QWORD
* REG\_QWORD\_LITTLE\_ENDIAN
* REG\_SZ

### Exercise

```
Set-NetFirewallProfile -Profile Public -Enabled False
​
Set-Location "C:\Users\htb-student\Desktop"
New-Item -ItemType Directory -Name "Company Data"
New-SmbShare -Path "C:\Users\htb-student\Desktop\Company Data" -Name "Company Data"
Get-SmbShare
​
Set-Location "Company Data"
New-Item -ItemType Directory -Name "HR"
​
New-LocalUser -Name "jim" -PasswordNeverExpires:$false
​
New-LocalGroup -Name "HR"
Add-LocalGroupMember -Name "HR" -Member "jim"
​
​
Get-Acl -Path "C:\Users\htb-student\Desktop\Company Data\HR"
​
Get-LocalUser -Name "jim" | Select-Object sid
Get-LocalGroup -Name "HR" | Select-Object sid
Get-Service -DisplayName "Win*"
​
```
