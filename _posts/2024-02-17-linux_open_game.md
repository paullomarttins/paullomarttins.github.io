---
layout: post
author: Paulo Martins
title: Um retro game console de código aberto
---

Sempre gostei muito de jogos antigos, desde criança quando comecei a ter acesso a computadores(sem internet) a luta para conseguir rodar alguns jogos com o hardware da época era grande, então surgiram os emuladores de jogos retro. Esses emuladares eram incríveis pois mesmo com uma máquina bem modesta não havia problemas para executar esses jogos, lembro da primeira vez que consegui um CD-ROM com mais de 900 jogos de Super Nintendo, fiquei louco com isso, embora já jogava outros jogos como Duke Nukem 3D e DOOM que já tomavam horas do meu dia. 

![Game Console](/assets/game.jpg "Game Console Open Source")

Hoje em dia temos várias formas de jogar esses jogos retro através de consoles portáteis, no próprio computador, consoles USB e muitos outros. Recentemente adquiri um Game Console Open Source que roda o **RetroArch**, já publiquei um artigo aqui sobre o [RetroPie](https://paullomarttins.github.io/retropie) que se baseia também no [RetroArch](https://www.retroarch.com/). O sistema que é executado nesse game console é o [**ArkOS**](https://github.com/christianhaitian/arkos/wiki) livre e de código aberto baseado no Ubuntu 19.10, seu fonte está no GitHub, o modelo do console é o R35S equipado com o processador Cortex-A7 1,5GHz, tela de 640*480 IPS e 96Gb de espaço interno que é dividido em dois Micro SD card.    

O barato desses consoles é que já vem pronto em termos de hardware, mas isso não impede de montar seu próprio console com algum modelo de [Raspberry Pi](https://www.raspberrypi.com/) existente no mercado. Como de costume já resolvi fazer algumas mudanças no meu game console, a primeira delas foi substituir os SD cards pois os que vem não é de boa qualidade e podem dar defeito muito rápido, então comprei dois micro sd da Sandisk de 32Gb e 64Gb. A segunda mudança é a versão do sistema operacional dele, por padrão ele vem uma versão de 2022, no github do mantenedor tem o link da versão mais atual e de um fork o ArkOS do [**AeolusUX**](https://github.com/AeolusUX/ArkOS-R3XS) que é o que irei usar no meu game console, essa verão vem com várias melhorias e sua versão mais atual é desse ano.

O processo para instalação é muito simples, na própria [_wiki_](https://github.com/christianhaitian/arkos/wiki) no github você encontra todos esses links e o passo a passo de como atualizar/substiuir o sistema. Minha opinião sobre esse game console é que vale a pena, os jogos na maioria dos emuladores rodam bem liso principalmente os de PSP e Playstation, boa bateira e muitas horas de diversão e nostalgia.  

- Fonte: [ArkOS](https://github.com/christianhaitian/arkos/wiki)
- Guia instalação: [Wiki](https://github.com/christianhaitian/arkos/wiki)
