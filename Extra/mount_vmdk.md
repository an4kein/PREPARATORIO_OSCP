Extrair .7z em linha de comando.

`p7zip -d devt-improved.7z`

Install [libguestfs-tools] - libguestfs, biblioteca e ferramentas para acessar e modificar imagens de disco da VM

`apt install libguestfs-tools`

Crie um dir em /mnt/

`mkdir /mnt/vmdk`

Monte a imagem usando o [guestmount] com permissao de escrita

`guestmount -a development-improved-disk1.vmdk -i --rw /mnt/vmdk`


Reference: 
https://stackoverflow.com/questions/22327728/mounting-vmdk-disk-image
http://manpages.ubuntu.com/manpages/trusty/man1/guestmount.1.html
