---
layout: post
title: ADBlock Pi - Dando adeus às propagandas
author: Paulo Martins
published: true
---

Nada pior do que esses malditos pop-up de propagandas estampando sua tela sempre que você visita um site ou clica em algum link, mas para isso existem várias extensões para os navegadores atuais que possibilitam o bloqueio dessas propagandas. A questão é que essas extensões podem sempre trazer algo a mais do que sua finalidade proposta, como sempre são ferramentas gratuitas, mas existe um preço por isso e seus dados de acesso podem ser o preço a ser pago, não que isso seja algo ruim ou invasivo para algumas pessoas, mas eu prefiro que meus dados permaneçam comigo onde é o seu lugar.

Com toda essa treta que não estou a fim de entrar em detalhes, para solucionar nossos problemas existe o [[https://pi-hole.net/|Pi-Hole]] no qual já havia testado algumas vezes em minha rede doméstica, porém nunca coloquei para rodar definitivamente. Andei fazendo algumas mudanças em minhas máquinas como upgrades já necessários, então meu laptop antigo (Frankenstein) ganhou uma função bem legal onde se tornou um servidor de máquinas virtuais e serviços. Como preciso acessar o ambiente da firma para trabalhar ele me ajuda muito com uma maquina já configurada nele, assim não vou consumir os recursos do meu desktop com máquina virtual, nele também hospedo minha nuvem local e o Pi-Hole dentre outras maquinas para testes.

O Pi-Hole como no site diz: **“É um sumidouro de DNS que protege seus dispositivos de conteúdo indesejado, sem instalar nenhum software do lado do cliente.”**, pela tradução direta by Google. O processo de instalação dele é mais do que simples, [veja a ducumentação](https://docs.pi-hole.net/main/basic-install/). No meu caso eu criei um conteiner LXC com Ubuntu Server 20.04 e executei o script de instalação:

```bash
root@anton:~# curl -sSL https://install.pi-hole.net | bash
```

Agora pronto só aguardar a finalização da instalação, claro no meio do caminho ele vai pedir para configurar algumas coisas, como nome da máquina, ip estático ou dinâmico, algum serviço de DNS que ele te dará opções de escolha e outras coisitas a mais, nada de tão misterioso assim.

Terminado o processo de instalação você vai acessar via browser com o ip que você configurou na instalação o painel admisnitrativo do Pi-Hole. Depois que fizer o login verifique se o serviço está ativo, agora você pode realizar um teste em sua rede interna mudando o DNS do seu roteador, ou você pode testar na sua maquina antes, no meu caso eu testei logo na rede.

Então é isso, estou pesquisando mais sobre as funcionalidades e listas de bloqueios para implementar no meu servidor, lembrando que Youtube é fracasso pois os domínios das propagandas mudam bastente então, sem sucesso, mas no restante é adeus às propagandas.

Minha configuração foi realizada em um laptop que nunca desligo, porém o ideal mesmo é que seja feito em um Raspberry Pi por conta da sua disponibilidade. Lembre-se que se seu servidor desligar o acesso a internet se vai junto com ele pois no roteador só terá um servidor DNS configurado.

Faça bom uso do Pi-Hole cool 8-). . .