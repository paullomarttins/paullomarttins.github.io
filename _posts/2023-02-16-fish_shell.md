---
title: Finalmente, um shell de linha de comando para os anos 90
published: true
---

Passei a utilizar o [Fish Shell](https://fishshell.com/) como padrão, o que é legal neste shell interativo é sua facilidade de utilização e todos os recursos disponíveis nele como a auto-sugestão. Ele está disponível para download na maioria dos repositórios das distribuições Linux, mas para obter a última versão é aconselhável baixar e instalar via PPA ou o pacote deb no [Launchpad](https://launchpad.net/~fish-shell/+archive/ubuntu/release-3/+packages), na documentação você pode encontrar como realizar esse procedimento.

```bash
user@np-350x~> sudo apt-add-repository ppa:fish-shell/release-3

user@np-350x~> sudo apt update

user@np-350x~> sudo apt install fish
```
Aprendi a gostar do Fish muito rápido e realmente ele me ajuda muito, além de ser bem fácil de configurar, claro ainda estou descobrindo muita coisa. Ele possui uma página web de configuração onde é possível definir algumas coisas como cor, estilo do prompt, suas funções, ver o histórico do shell e etc.

Eu defini o Fish como meu shell padrão no sistema, mas caso queira utilizar em paralelo com o bash basta digitar fish no terminal e ele será utilizado enquanto estiver com a sessão aberta. A maneira que usei para defini-ló como padrão foi alterar a linha 40 do /etc/passwd, onde está bash alterei para fish, se você utiliza o Ubuntu Mate nas configurações de usuários, no modo avançado dá para mudar o shell padrão.

Para acessar a página de configuração web digite:

```bash
user@np-350x~> fish_config
```

Essa é a tela de configurações web que será aberta no browser:

Existe outras coisas bem legais que podem ser feitas com o Fish, como definir frases e textos engraçados ou desenhos quando o shell é iniciado. Bom isso é mais perfumaria mas é bem legal ir a fundo e configurar a seu gosto, para incluir essa funcionalidade vamos usar a função [fish_greeting](https://fishshell.com/docs/current/cmds/fish_greeting.html#cmd-fish-greeting).

Primeiro vamos acessar o caminho etc/fish/config.fish e vamos chamar nossa função, por padrão na documentação já tem um código pronto que é só copiar e colar. Como quero utilizar frases e desenhos na chamada da função estou utilizando dois recursos chamados de [fortune](https://wiki.debian.org/fortune) e [cowsay](https://helpmanual.io/man6/cowsay).

Instale os dois com o comando abaixo, caso ainda não tenha instalado:

```bash
user@np-350x~> sudo apt install fortunes cowsay
```

O Fortune é uma coleção de frases, textos e piadas dos desenvolveres e o Cowsay são desenhos interativos em ASCII e podem ser usados juntos para gerar imagen e texto. Abaixo tem o código que usei para para gerar uma frase e desenho em modo aleatório.

```bash
function fish_greeting
    echo Hi friend!!!
    echo The time is (set_color yellow; date +%T; set_color normal) and this machine called (hostname).
    echo Uptime: (uptime)

    set -l teste (random choice {tux, vader, gnu, suse})
    if which fortune > /dev/null ^ /dev/null
        fortune -s | cowsay -f $teste
    else
        echo Algo de errado não está certo....
    end
end
```

Agora toda vez que abrir o terminal ou executar o fish será retornado uma frase e desenho aleatório. Fish Shell é bem legal!
