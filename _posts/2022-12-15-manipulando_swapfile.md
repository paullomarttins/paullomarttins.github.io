---
title: Manipulando arquivo swapfile
published: true
---

Quando é feita uma instalação padrão do Ubuntu, ela automaticamente cria um arquivo de troca swapfile, ao invés de criar uma partição a mais.Por padrão o arquivo criado é de apenas 2Gb, claro a depender da quantidade de memória RAM que sua maquina disponha, você nem iria precisar de swap. Minha maquina tem 8Gb, então vou alterar meu aquivo swapfile de 2Gb para 16gb, ou seja, vou alterar para o dobro de memoria RAM que tenho.

Para conferir qual o tamanho da swap:

```bash
root@np340X:~$ free -h
total usada livre compart. buff/cache disponível
Mem.: 7,7Gi 2,4Gi 966Mi 540Mi 4,4Gi 4,5Gi
Swap: 2Gi 17Mi 2Gi
```

Agora preciso é desativar o swapfile para poder iniciar a brincadeira:

```bash 
root@np340X:~$ swapoff /swapfile
```

Depois de desativar, vou usar o “dd” para formatar o swapfile, como o tamanho que vou utilizar agora:

```bash
root@np340X:~$ dd if=/dev/zero of=/swapfile bs=1M count=16000 status=progess
```

Após o procedimento finalizar, configure o arquivo como um “arquivo de troca”:

```bash
root@np340X:~$ mkswap /swapfile
```

Agora ative o swapfile:

```bash
root@np340X:~$ swapon /swapfile
```

Fim! Pode conferir o tamanho:

```bash
root@np340X:~$ free – m
total usada livre compart. buff/cache disponível
Mem.: 7873 2430 944 547 4498 4599
Swap: 16000 0 16000
```

É isso, valeu e boa diversão! ;-)