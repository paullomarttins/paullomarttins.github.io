---
title: Sincronizando pasta local para nuvem com Rclone
published: true
---

Definitivamente resolvi encerrar minha relação com o Dropbox, depois de muito tempo juntos, enfim, estamos dando um ponto final. Já não usava com muita frequência, sempre fui mais a favor de armazenamento local principalmente quando envolve dados mais privados. Existem vários motivos para essa decisão assim como encerrei minha conta no Mega a alguns anos.

Vivemos em uma era em que a privacidade na internet é posta a prova, então onde seus dados ficarão armazenados é algo para se preocupar e a depender do que você irá armazenar na sua nuvem poderá ser tornar um grande problema, eu por outro lado não mantenho informações confidenciais em nuvem, basicamente armazeno dados que preciso ter em todo lugar e de uma forma mais rápida.

Já possuo o Drive que disponibiliza 17Gb de storage, também tenho uma conta no Box que por sinal nunca utilizei de forma alguma, a sua conta free disponibiliza 10Gb de storage. Contudo, os dois serviços não dispõem de clientes oficiais para Linux, existem algumas ferramentas pagas que podem atender caso queira pagar, então pesquisando por aí encontrei o Rclone (Rclone - command line program to sync files and directories to and from cloud storage) que pode ser instalado dos repositórios do Ubuntu.

Na documentação da ferramenta você encontra todo o processo de configuração que por sinal é mamão com açúcar, tente dar um man rclone no terminal para listar toda a documentação. Vou configurar minha conta do Box pois o Drive eu consigo sincronizar com o serviço que existe no Kubuntu.

Para inciar a configuração digite:

```bash
rclone config
```

A partir dai é só seguir todos os passos para a configuração referente ao serviço que você escolheu.
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


