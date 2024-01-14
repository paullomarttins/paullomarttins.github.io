---
layout: post
title: LVM – Criando um HDD híbrido
author: Paulo Martins
published: true
---

Desde que passei a utilizar o lvm em meus computadores tudo se tornou possível, exagero meu? Não, com certeza o [lvm](https://wiki.ubuntu.com/Lvm) não é exagero quando se trata de desempenho e praticidade com a manipulação do seu filesystem. Recentemente realizei um update em meu desktop, que contarei depois aqui, então resolvi implementar uma solução ótima que o lvm proporciona. Em minhas pesquisas no uso de lvm conheci o [lvmcache](https://manpages.ubuntu.com/manpages/xenial/man7/lvmcache.7.html) - **LVM caching**, que resumidamente, possibilita o uso de um disco rápido como cache para um disco mais lento.

Lógico que usar somente um SSD como disco principal para seu sistema o resultado será muito superior, isso não tenho dúvida, embora as soluções em HDD híbidos no mercado são bem atraentes atualmente. Mas ai vem a velha questão, porque converter um HDD para híbrido se eu posso somente utilizar o SSD como disco principal e o HDD para meus dados, ou seja, minha partição /home ou backup, pensando no aproveitamento de espaço do disco mais lento não pensei duas vezes, então quis testar essa maravilha de recurso que o lvm me proporciona. Como eu já tinha um HD de 500G em meu desktop com meu sistema instalado e já configurado com lvm, só precisei adicionar o meu SSD que estava em uso no meu laptop.

Como eu já havia criado uma partição no disco, no exemplo abaixo, meu retorno é esse:

```bash
root@c137:~# fdisk -l /dev/sdb
Disk /dev/sdb: 223,58 GiB, 240057409536 bytes, 468862128 sectors
Disk model: WDC WDS240G2G0B-
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 50E20E4E-0ACC-E646-81EA-8ADC8C466812

Device Start End Sectors Size Type
/dev/sdb1 2048 468862094 468860047 223,6G Linux LVM
```

Agora irei criar a partição lvm e extender meu Volume Group com meu novo disco adicionado, o SSD.

```bash
root@c137:~# pvcreate /dev/sdb1

 Physical volume "/dev/sdb1" successfully created.
```

```bash
root@c137:~# vgextend vgubuntu /dev/sdb1 

 Volume group "vgubuntu" successfully extended
```

Feito isso minha configuração fica assim:

```bash
root@c137:~# lsblk 

NAME MAJ:MIN RM SIZE RO TYPE MOUNTPOINT

sda 8:0 0 465,8G 0 disk 

├─sda1 8:1 0 512M 0 part /boot/efi

├─sda2 8:2 0 2G 0 part /boot

└─sda3 8:3 0 463,3G 0 part  

  ├─vgubuntu-home 253:2 0 300G 0 lvm /home

  |─vgubuntu-root 253:6 0 100G 0 lvm /

  ├─vgubuntu-swap 253:7 0 8G 0 lvm [SWAP]

  ├─vgubuntu-tmp 253:8 0 5G 0 lvm /tmp

  └─vgubuntu-var 253:9 0 50,3G 0 lvm /var

sdb 8:16 0 223,6G 0 disk 

└─sdb1 8:17 0 223,6G 0 part 
```

De acordo com a documentação do lvmcache é dessa forma que ele trabalha:

>“O tipo de volume lógico do cache usa um LV pequeno e rápido para melhorar o desempenho de um LV grande e lento. Ele faz isso armazenando os blocos  usados com frequência no LV mais rápido.
>
>O LVM se refere ao pequeno LV rápido como um cache pool LV. O grande LV lento é chamado de origin LV. Devido aos requisitos do dm-cache (o driver do kernel), o LVM divide ainda mais o pool de cache LV em dois dispositivos - os dados de cache LV e os metadados de cache LV. Os dados do cache LV é onde as cópias dos blocos de dados são mantidas do origin LV para aumentar a velocidade. O cahe metadata LV contém as informações contábeis que especificam onde os blocos de dados estão armazenados (por exemplo, no origin LV ou nos dados de cache LV). Os usuários devem estar familiarizados com esses LVs se desejam criar os melhores e mais robustos volumes lógicos em cache. Tudos esses LVs associados devem estar no mesmo VG.”

===== Cache Terms =====

```bash
origin LV OriginLV large slow LV

cache data LV CacheDataLV small fast LV for cache pool data

cache metadata LV CacheMetaLV small fast LV for cache pool metadata

cache pool LV CachePoolLV CacheDataLV + CacheMetaLV

cache LV CacheLV OriginLV + CachePoolLV
```

Deixando de conversa, vamos criar os LV necessários para o cache. Neste primeiro caso temos a criação do **cache metadate LV**:

```bash
root@c137:~# lvcreate -n lv_meta -L 253M vgubuntu /dev/sdb1
```

Criando o **cache data LV**:

```bash
root@c137:~# lvcreate -n lv_cache -l 99%PVS vgubuntu /dev/sdb1
```

Perfeito com os dois LV criados, preciso agora combinar os dois criando um cache pool LV e também será preciso escolher o cache mode. Existem dois modos o padrão "writethrough" e o segundo “writeback”. No meu desktop eu irei utilizar o "writethrough" pois ele garante a gravação do dado sem correr o risco de perda, embora o “writeback” tenha um ganho de desempenho melhor mas não garante que o dado seja gravado em caso de uma queda de energia ou falha no disco.

```bash
root@c137:~# lvconvert --type cache-pool --cachemode writethrough --poolmetadata vgubuntu/lv_meta vgubuntu/lv_cache
```

Por fim vamos criar o cache, aqui estou atribuíndo o cache para minha partição /root onde os programas são instalados e executados:

```bash
root@c137:~# lvconvert --type cache --cachepool vgubuntu/lv_cache vgubuntu/root
```

Consultando o lvdisplay tudo ficou assim:

```bash
root@c137:~# lvdisplay

--- Logical volume ---

LV Path /dev/vgubuntu/root
LV Name root
VG Name vgubuntu
LV UUID OHxRqF-ksyM-cBkU-GKyh-mscT-vNpz-PUICCN
LV Write Access read/write
LV Creation host, time kubuntu, 2021-06-26 12:49:02 -0300
LV Cache pool name lv_cache_cpool
LV Cache origin name root_corig
LV Status available
# open 1
LV Size 200,00 GiB
Cache used blocks 1,15%
Cache metadata blocks 4,68%
Cache dirty blocks 0,00%
Cache read hits/misses 65811 / 18405
Cache wrt hits/misses 23429 / 22394
Cache demotions 0
Cache promotions 6384
Current LE 51200
Segments 1
Allocation inherit
Read ahead sectors auto
- currently set to 256
Block device 253:6
```

Para remover o cache do lvm basta desabilitá-lo da partição que ele foi criado:

```bash
root@c137:~# lvconvert --uncache vgubuntu/root
```

A pergunta é, ganhei algum desempenho com tudo isso? Sim notei um melhor desempenho em meu desktop, não tanto quanto no laptop, mas para ser justo o HD dele é muito lento mesmo, 5400rpm. Não fiz nenhum comparativo de desempenho ainda, é muito cedo e estou testando bastante, mas é claro que dá para notar a diferença se você conhece sua máquina.

O lvmcache me ajudou a aproveitar o espaço dos meus HDDs. No mercado já existem soluções prontas para HDD híbrido sem que seja preciso toda essa configuração embora muito interessante e diga-se de passagem maravilhosa.

Free Software is way!!