---
title: Gravando pendrive bootavél com DD
published: true
---

Existem várias ferramentas para para criação de pendrive bootavél e/ou para criar imagens, mas nada melhor 
que o bom e velho DD que na verdade, é uma ferramenta muito poderosa para copiar e converter arquivos. Nesse caso estou utilizando para criar um pendrive de boot do [Kubuntu](https://kubuntu.org/), primeiro baixe a imagem .iso no site da distribuição. 

Vamos localizar o pendrive conectado na máquina:

```bash
root@np-350X:~# fdisk -l
Disco <wrap hi>/dev/sdc: 3,63 GiB</wrap>, 3880452096 bytes, 7579008 setores
Disk model: USB DISK 2.0
Unidades: setor de 1 * 512 = 512 bytes
Tamanho de setor (lógico/físico): 512 bytes / 512 bytes
Tamanho E/S (mínimo/ótimo): 512 bytes / 512 bytes
Tipo de rótulo do disco: dos
Identificador do disco: 0x15f006ae
Dispositivo Inicializar Início Fim Setores Tamanho Id Tipo
/dev/sdc1 * 0 5303231 5303232 2,5G 0 Vazia
/dev/sdc2 4222640 4230575 7936 3,9M ef EFI (FAT-12/16/32)
/dev/sdc3 5304320 7579007 2274688 1,1G 83 Linux
```

No meu caso irei utilizar o dispositivo **/dev/sdc**. 

```bash
root@np-350X:~# dd if=kubuntu-20.04-desktop-amd64.iso of=/dev/sdc bs=4M status=progress && sync
2353004544 bytes (2,4 GB, 2,2 GiB) copiados, 350 s, 6,7 MB/s
561+1 registros de entrada
561+1 registros de saída
2354036736 bytes (2,4 GB, 2,2 GiB) copiados, 349,845 s, 6,7 MB/s
```

Finalizado o processo agora é só realizar um teste, reiniciar a máquina e iniciar a partir do pendrive.

Os parâmetros usados no comando acima:

**bs** = Defina os tamanhos dos blocos de entrada e saída como bytes.

**status** = Com a inclusão do “progress” você pode acompanhar o progresso da cópia.

**sync** = Permite copiar tudo usando E / S sincronizada.
 

Você pode consultar mais com dd –help, no man dd ou na documentação da ferramenta neste [link](https://www.gnu.org/software/coreutils/manual/html_node/dd-invocation.html#dd-invocation).