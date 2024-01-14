---
layout: post
title: Criptografar disco inteiro GNU/Linux
author: Paulo Martins
published: true
---

Minha máquina de uso diário é o meu laptop, apesar de ter um desktop em casa utilizo ele para testes de servidor e outras  gambiarras, ou seja, meu laboratório para testes. Como tenho costume de sempre transportar o laptop para cima e para baixo, fazer uso de criptografia de disco é uma boa idéia.

Na maioria das distribuições atuais existe a opção de criptografia na instalação padrão, então você escolher com facilidade a opção de uma instalação já criptografada, mas não é isso o que eu quero. Minha intenção é ter controle sobre o que irei criptografar e o quanto de espaço irei usar em minhas partições principalmente, coisa que a instalação automática não lhe proporcionará.
 
A ferramenta para essa empreitada é o LUKS com LVM o mesmo que a instalação automática do Ubuntu utiliza neste caso, então pesquisei na documentação do [Ubuntu](https://help.ubuntu.com/community/Full_Disk_Encryption_Howto_2019) para começar. No fórum da RedHat nesse [link](https://www.redhat.com/sysadmin/disk-encryption-luks) e na wiki do [ArchLinux](https://wiki.archlinux.org/index.php/Dm-crypt/Encrypting_an_entire_system) você tem um material bem completo para começar a estudar sobre isso também. 

**1.** A primeira coisa é inciar o Ubuntu em modo live para podermos definir qual disco iremos utilizar, faça as formatações das unidades de acordo com sua instalação via terminal:

```bash
root@np340X:~# fdisk -l /dev/sda 
 Disco /dev/sda: 223,58 GiB, 240057409536 bytes, 468862128 setores 
 Disk model: KINGSTON SA400S3 
 Unidades: setor de 1 * 512 = 512 bytes 
 Tamanho de setor (lógico/físico): 512 bytes / 512 bytes 
 Tamanho E/S (mínimo/ótimo): 512 bytes / 512 bytes 
 Tipo de rótulo do disco: gpt 
 Identificador do disco: E2607B28-B351-4D10-A310-CF789C8DBD5C 
 
 Dispositivo  Início       Fim   Setores Tamanho Tipo 
 /dev/sda1      2048   1050623   1048576    512M Sistema EFI 
 /dev/sda2   1050624   3147775   2097152      1G Linux sistema de arquivos 
 /dev/sda3   3147776 468862094 465714319  222,1G Linux sistema de arquivo
```

**2.** O volume /dev/sda3 será onde irei configurar o LVM e o LUKS: 

```bash
sudo cryptsetup -y -v luksFormat /dev/sda3
sudo cryptsetup open /dev/sda3 sda3_crypt
sudo mkfs.ext4 /dev/mapper/sda3_crypt
```

**3.** Após a criação da partição e formatação do file-system (coisa que você pode fazer na instalação gráfica), vou criar meus volumes LVM. Confira o man de pvcreate vgcreate lvcreate:

```bash
sudo pvcreate /dev/mapper/sda3_crypt
sudo vgcreate vgubuntu /dev/mapper/sda3_crypt
sudo lvcreate -n root -L 90G vgubuntu
sudo lvcreate -n home -l 100%FREE vgubuntu
```

**4.** Terminado os passos acima já podemos ir para o modo de instalação gráfica e começar a brincadeira. Siga todos os passos padrões de instalação do Ubuntu e quando chegar na etapa de formatação escolha o modo "manual", lá você irá indicar as partições onde será instalado o sistema. 

Observe com atenção quais partições irá utilizar e suas necessidades, por exemplo, a partição efi (/dev/sda1), boot (/dev/sda2) e as partições de root (/dev/mapper/vgubuntu-root) e home (/dev/mapper/vgubuntu-home). 

**5.** Ao completar a instalação continue em modo de teste do Ubuntu e volte ao terminal para montar as partições raiz do sistema:

```bash
sudo mount /dev/mapper/vgubuntu-root /mnt
sudo mount /dev/sda2 /mnt/boot
sudo mount --bind /dev /mnt/dev
sudo chroot /mnt
# mount -t proc proc /proc
# mount -t sysfs sys /sys
# mount -t devpts devpts /dev/pts 
```

**6.** Agora ainda em modo chroot, crie o arquivo /etc/crypttab/ e adicione os volumes LUKS para serem montados na inicialização. Use o blkid para identificar o UUID dos volumes:

```sudo blkid </dev/sda3>```

No arquivo **/etc/crypttab** cole os dados abaixo, em UUID cole a saída do blkid:

```bash
# <target name> <source device> <key file> <options>

sda3_crypt UUID=UUID_ROOT> none luks,discard
```

**7.** Por fim, atualize o initramfs e reinicie o sistema:

``` update-initramfs -k all -c```

Até aqui isso é tudo, você está com seu sistema criptografado logo ao iniciar irá pedir a senha definida durante a instalação para descriptografar seu disco.