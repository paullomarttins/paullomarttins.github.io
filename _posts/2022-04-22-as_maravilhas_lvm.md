---
layout: post
author: Paulo Martins
title: As maravilhas do LVM
---

Não é novidade que o **LVM** vai muito além de ser maravilhoso, isso é fato! Continuando minha saga com lvm, hoje precisei realizar uma mudança nas minhas partições lvm (atividades do recesso de Réveillon) pois irei mudar algums cositas em meu SO e para começar, uma delas será renomear o meu grupo de volumes e alguns volumes lógicos, o procedimento não é difícil e a documentação do próprio lvm pode ajudar nisso.

Primeiro vou renomear o meu grupo de volume principal:

```bash 
sudo vgrename -v vgubuntu storaged 
```

Note que o nome do grupo de volume já foi alterado para storaged (novo nome):

Após essa mudança é preciso alterar os nomes dos volumes no **fstab**, pois lá ainda vai constar o nome do grupo anterior. Essa alteração é fundamental ou ao reiniciar a máquina essas partições não serão montadas.

Alterada todas as entradas do fstab que constava o nome do grupo anterior, precisamos agora alterar as entradas no arquivo de configuração do **grub**, neste também é preciso alterar todas as entradas que possuirem o grupo anterior.

```bash 
sudo nano /boot/grub/grub.cfg 
```

Agora tudo ok, tudo alterado vamos atualizar as configurações do grub?

```bash 
sudo update-initramfs -k all -c    
```

Nops, me deparei com um erro claro, como haviamos alterado o nome do grupo e as configurações do fstab, precisamos montar novamente o root para a nova entrada:

```bash 
sudo mount /dev/mapper/storaged-root /mnt

sudo mount /dev/sda2 /mnt/boot

sudo mount --bind /dev /mnt/dev

sudo chroot /mnt

mount -t proc proc /proc

mount -t sysfs sys /sys

mount -t devpts devpts /dev/pts   
```

Por fim esse foi meu esquema de montagem, agora podemos enfim atualizar o initramfs:

```bash
sudo update-initramfs -k all -c
```

Sucesso, coisinha fácil mas mostra as facilidades que o LVM pode proporcionar né! A partir daqui vou alterar mais algumas coisas, qualquer novidade talvez eu inclua neste artigo.

Gosto bastante de trabalhar com lvm e entender as possibilidades que posso ter com esse recurso incrível.