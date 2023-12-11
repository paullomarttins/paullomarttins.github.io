---
layout: post
author: Paulo Martins
title: Cawbird um fork do Corebird
---

Já utilizei muito o [Corebird](https://corebird.baedert.org/) como client do Twitter, mas depois que foi descontinuado devido as mudanças na API do próprio Twitter não houve, na minha opinião, nenhum client a sua altura.

Recentemente descobrir um projeto chamado [Cawbird](https://github.com/IBBoard/cawbird), que na verdade é um fork do Corebird, minha primeira impressão foi bem positiva, ele me lembra bastante o Corebird.

Se quiser acompanhar o projeto veja a página dele no GitHub, lá também contém as várias formas de [instalação](https://software.opensuse.org//download.html?project=home%3AIBBoard%3Acawbird&package=cawbird) bem como suas limitações. É um projeto em desenvolvimento, então muita coisa nova vem por ai.

A forma que instalei no Ubuntu 20.04 foi a seguinte:

```bash
echo 'deb http://download.opensuse.org/repositories/home:/IBBoard:/cawbird/xUbuntu_20.04/ /' | sudo tee /etc/apt/sources.list.d/home:IBBoard:cawbird.list

curl -fsSL https://download.opensuse.org/repositories/home:IBBoard:cawbird/xUbuntu_20.04/Release.key | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/home:IBBoard:cawbird.gpg > /dev/null

sudo apt update

sudo apt install cawbird
```

É isso, o Cawbird está sendo sempre atualizado com novas correções.