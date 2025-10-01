---
layout: post
title: Sincronizando pasta local para nuvem com Rclone
author: Paulo Martins
published: true
---

Definitivamente, decidi encerrar minha relação com o Dropbox. Depois de muito tempo juntos, finalmente estamos dando um ponto final. "Eu não usava com muita frequência. Sempre fui mais a favor do armazenamento local, especialmente quando se trata de dados mais privados." Existem vários motivos para essa decisão, assim como o fato de que encerrei minha conta no Mega há alguns anos.

Vivemos em uma era em que a privacidade na internet é colocada à prova. Assim, onde seus dados serão armazenados é algo para se preocupar. Dependendo do que você vai guardar na nuvem, isso pode se tornar um grande problema. Eu, por outro lado, não mantenho informações confidenciais na nuvem. Basicamente, armazeno dados que preciso acessar de qualquer lugar e de forma mais rápida.

Já possuo o Google Drive, que oferece 17 GB de armazenamento. Também tenho uma conta no Box, que, por sinal, nunca utilizei. A conta gratuita do Box disponibiliza 10 GB de armazenamento. No entanto, os dois serviços não têm clientes oficiais para Linux. Existem algumas ferramentas pagas que podem ajudar, caso você esteja disposto a pagar. Pesquisando, encontrei o Rclone (Rclone - programa de linha de comando para sincronizar arquivos e diretórios com o armazenamento em nuvem), que pode ser instalado a partir dos repositórios do Ubuntu.

Na documentação da ferramenta, você encontra todo o processo de configuração, que é muito fácil. Tente usar o comando "man rclone" no terminal para listar toda a documentação. Vou configurar minha conta do Box, pois consigo sincronizar o Drive com o serviço que existe no Kubuntu.

Para inciar a configuração digite:

```bash
rclone config
```

A partir daí é só seguir todos os passos para a configuração referente ao serviço que você escolheu.
Após toda a configuração posso listar a minha pasta remota:

```bash
rclone lsd remote:
```

Para listar os arquivos da pasta remota:

```bash
rclone ls remote:
```

E por fim para sincronizar um pasta local para sua pasta remota:

```bash
rclone copy /home/source remote:Box
```

Bom, acho que é tudo. Vou ver uma maneira de automatizar a sincronização uma vez ao dia.
