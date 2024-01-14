---
layout: post
author: Paulo Martins
title: Liberdade no Design - Penpot
---

O [Penpot](https://penpot.app/) é uma alternativa às ferramentas propritárias como o **Figma** para design, totalmente livre e de código aberto. Com o **Penpot** é possível criar ou melhorar o design da sua aplicação e dos seus dashboads, caso você trabalhe com analise de dados assim como eu.

_"Penpot is the Open-Source Design & Prototyping Tool for Product Teams"_

Para instalar o Penpot é muito fácil, com a possibilidade de instalação via Docker se torna ainda mais tranquilo. Abaixo vou mostar o que é preciso para essa instalação, caso ainda não possua o **Docker** na sua máquina, aconselho seguir o passo a passo da instalação na documentação [link](https://docs.docker.com/get-docker/):

Aqui vamos obter o _docker-compose.yaml_ que é o arquivo de configuração que subirá nosso container:

```bash
~ $ curl -o docker-compose.yaml https://raw.githubusercontent.com/penpot/penpot/main/docker/images/docker-compose.yaml
```

Após baixar o arquivo de configuração, iremos subir o container:

```bash
~ $ docker compose -p penpot -f docker-compose.yaml up -d
```

Finalizando o deploy do container acesse o Penpot através do link [http://localhost:9001](http://localhost:9001).

Para parar o container execute:

```bash
~ $ docker compose -p penpot -f docker-compose.yaml down
```

Com a configuração padrão do arquivo _docker-compose.yaml_ as opções de criação de novos usuários já estão configuradas, para criar um novo usuário você poderá utilizar a página de registro ao acessar pela primeira vez a página inicial [http://localhost:9001](http://localhost:9001). Como alternativa a criação de novos usuários poderá ser feita via terminal com docker como no exemplo abaixo:

```bash
~ $ docker exec -ti penpot-penpot-backend-1 python3 ./manage.py create-profile
Email: teste@teste.org
Fullname: Paulo
Password: 
Created: teste@teste.org
```

Para deletar um usuário use o comando _delete-profile_:

```bash
~ $ docker exec -ti penpot-penpot-backend-1 python3 ./manage.py delete-profile
Email: teste@teste.org
Deleted
```

Quem já utiliza outras ferramentas proprietárias similares ao **Penpot** não terá muita dificuldade na sua utilização, com muitas funcionalidades e recursos disponivéis o Penpot não fica para trás de nenhuma forma. Segue o [link](https://help.penpot.app/user-guide/) da documentação que é muito completa onde você poderá dar os primeiros passos nessa ferramenta incrível.

- Fonte: [https://penpot.app](https://penpot.app/)
- Guia instalação: [https://help.penpot.app/technical-guide/getting-started/](https://help.penpot.app/technical-guide/getting-started/)
- Documentação: [https://help.penpot.app/user-guide](https://help.penpot.app/user-guide/)