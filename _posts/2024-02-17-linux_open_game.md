---
layout: post
author: Paulo Martins
title: Um retro game console de código aberto
categories: retro games
tags: games
---

Sempre gostei muito de jogos antigos. Desde criança, quando comecei a ter acesso a computadores (sem internet), a luta para conseguir rodar alguns jogos com o hardware da época era grande. Então, surgiram os emuladores de jogos retrô. Esses emuladores eram incríveis, pois, mesmo com uma máquina bem modesta, não havia problemas para executar esses jogos. Lembro da primeira vez que consegui um CD-ROM com mais de 900 jogos de Super Nintendo. Fiquei animado com isso, embora já jogasse outros jogos, como Duke Nukem 3D e DOOM, que já tomavam horas do meu dia.

![Game Console](/assets/game.jpg "Game Console Open Source")

Hoje em dia, temos várias maneiras de jogar esses jogos retro, como em consoles portáteis, no computador, em consoles USB e em muitos outros dispositivos. Recentemente, adquiri um console de jogos de código aberto que roda o **RetroArch**. Já publiquei um artigo aqui sobre isso [RetroPie](https://paullomarttins.github.io/retropie) que se baseia também no [RetroArch](https://www.retroarch.com/). O sistema que é executado nesse console é o [**ArkOS**](https://github.com/christianhaitian/arkos/wiki) livre e de código aberto baseado no Ubuntu 19.10, seu fonte está no GitHub, o modelo do console é o R35S equipado com o processador Cortex-A7 1,5GHz, tela de 640*480 IPS e 96Gb de espaço interno que é dividido em dois Micro SD card.

O legal desses consoles é que eles já vêm prontos em termos de hardware. Mas isso não impede que você monte seu próprio console com algum modelo de [Raspberry Pi](https://www.raspberrypi.com/) existente no mercado. Como de costume, resolvi fazer algumas mudanças no meu console de jogos. A primeira delas foi substituir os cartões SD, pois os que vêm de fábrica não são de boa qualidade e podem apresentar defeito muito rápido. Então, comprei dois microSD da Sandisk, um de 32 GB e outro de 64 GB. A segunda mudança é a versão do sistema operacional. Por padrão, ele vem com uma versão de 2022. No GitHub do mantenedor, há um link para a versão mais atual e para um fork do projeto [**AeolusUX**](https://github.com/AeolusUX/ArkOS-R3XS) o qual irei usar no meu console, essa versão possui várias melhorias.

O processo para instalação é muito simples, na própria [_wiki_](https://github.com/christianhaitian/arkos/wiki). No GitHub, você encontra todos esses links e o passo a passo de como atualizar ou substituir o sistema. Na minha opinião, esse console de jogos vale a pena. A maioria dos emuladores roda bem, especialmente os de PSP e PlayStation. Tem uma boa bateria e oferece muitas horas de diversão e nostalgia.

- Fonte: [ArkOS](https://github.com/christianhaitian/arkos/wiki)
- Guia instalação: [Wiki](https://github.com/christianhaitian/arkos/wiki)
