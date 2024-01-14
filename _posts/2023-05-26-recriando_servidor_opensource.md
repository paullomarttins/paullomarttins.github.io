---
layout: post
title: Recriando servidor local open-source - Sucateando
author: Paulo Martins
published: true
---

Computadores podem durar muitos anos se forem bem zelados por seus donos, podem também ser úteis para vários projetos com a sua reutilização. Caso não saiba o que fazer com equipamentos velhos que não tenham mais utilidade para você, faça o descarte desse equipamento com sabedoria, existem postos de coletas para esse tipo de material.

> Problemas causados pelo descarte inadequado 

> - Este descarte é feito quando o equipamento apresenta defeito ou se torna obsoleto (ultrapassado). O problema ocorre quando este material é descartado no meio ambiente. Como estes equipamentos possuem substâncias químicas (chumbo, cádmio, mercúrio, berílio, etc.) em suas composições, podem provocar contaminação de solo e água. 

> - Além do contaminar o meio ambiente, estas substâncias químicas podem provocar doenças graves em pessoas que coletam produtos em lixões, terrenos baldios ou na rua. 

> - Estes equipamentos são compostos também por grande quantidade de plástico, metais e vidro. Estes materiais demoram muito tempo para se decompor no solo.

Bom, já tem um bom tempo que utilizo servidores locais para estudos, meus testes e pequenos projetos. Abaixo está meu pequeno grande **"monstrinho de Frankenstein"**, a história dele eu não faço idéia de como começou antes de ser encontrado, a muitos anos eu estava no interiror e um amigo meu tinha um galpão cheio de coisas eletrônicas sucateadas no canto, especificamente computadores e periféricos. Vasculhando as coisas eis que encontro esse laptop, sem bateria, sem memória RAM, sem HD e sem o LCD, acho que tinha a fonte também. Procurando mais, para suprir os periféricos que estavam faltando, encontrei HD, memória e um LCD de outro modelo que dava para adaptar nele, montando tudo a surpresa, a máquina estava boa, rodei vários testes e simplismente estava ótimo. 

Desde então fiz alumas adptações nele, como não trabalho com a parte física dele, tudo é via web ou terminal remoto, então ele fica paradinho aqui no canto. Eu rodo nele o [Proxmox](https://www.proxmox.com/en/), que é uma solução em servidores open-source baseado em Debian muito legal, com ele você pode executar maquinas virtuais e conteiners.

Até hoje ele funciona muito bem, nunca tive problemas com o hardware, fora o HD antigo que estava mais para lá do que para cá daí tive que substituir, fora isso ele roda perfeitamente com suas configurações singelas mas que me fornecem um bom ambinete para testes diversos com meus projetos.

Por padrão o Proxmox já vem totalmente pronto para o uso, mas é claro que existem algumas coisas que prefiro alterar para um melhor aproveitamento dos recursos adpatado ao meu laptop antigo. Um exemplo é a confiuração do Storage, ele assume pouco espaço na partição princiapal onde ficará armazenadas as imagens e conteiners baixados, é claro o espaço em disco alocado na instalação dependerá do tamanho do seu HD. 

Mas isso pode ser facilmente revertido porque ele utiliza LVM, então será mamão com açucar alterar o tamanho dessas partições e assim aproveitar melhor o espaço em disco do seu jeito.

```bash
root@solo:~# lsblk 
NAME MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
sda 8:0 0 465.8G 0 disk 
├─sda1 8:1 0 1007K 0 part 
├─sda2 8:2 0 512M 0 part /boot/efi
└─sda3 8:3 0 465.3G 0 part 
 ├─pve-swap 253:0 0 7G 0 lvm [SWAP]
 ├─pve-root 253:1 0 96G 0 lvm /
 ├─pve-data_tmeta 253:2 0 3.5G 0 lvm 
 │ └─pve-data 253:4 0 339.3G 0 lvm 
 └─pve-data_tdata 253:3 0 339.3G 0 lvm 
 └─pve-data 253:4 0 339.3G 0 lvm 

root@solo:~# cat /etc/pve/storage.cfg 
dir: local
 path /var/lib/vz
 content iso,vztmpl,backup

lvmthin: local-lvm
 thinpool data
 vgname pve
 content rootdir,images
```

Na instalação básica ele vem com [LVM-Thin](https://pve.proxmox.com/wiki/Storage:_LVM_Thin), que basicamente aloca dinamicamente o espaço que será ou não utilizado na VM, o provisionamento Thin ocupa apenas o que é usado, portanto, entre suas várias VMs, você poderia alocar a elas 50 GB de armazenamento total em uma unidade de 20 GB. mas você pode conferir mais profundamente sobre como o Thin funciona em sua [documentação](https://man7.org/linux/man-pages/man7/lvmthin.7.html). 

É um ótimo recurso, pois ele fará o uso adequado do espaço em disco para cada VM, mas existem problemas com isso, se por acaso você notar que está havendo um aumento do armazenamento Thin então ações devem ser tomadas, como o uso do "Discard" e uma emulação de SDD no disco da VM, tudo isso está na documentação do Proxmox. Abaixo um exemplo de espaço usado em disco com duas VM de 32 GB cada: (1.72% (6.27 GB of 364.35 GB)

Bom, particularmente prefiro desfazer o LVM-Thin e expandir a partição principal root alocando assim todo o espaço em disco dela. 

O título diz "Recriando servidor local" como disse antes, tive um problema com o HD dele e foi preciso recriar todo meu ambiente, a sorte que mantenho sempre um backup das VMs e conteiners bem guardados.