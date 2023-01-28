---
layout: post
title:  docker - uma introdução didática - parte 1
date:   2023-01-28 01:00:00
description: introdução à conteinerização com docker e docker-compose, conceitos básicos, comandos e exemplos
tags: ["docker", "docker-compose", "containerization"]
language: pt-br
---
## Introdução

Este material traz um compilado de tópicos introdutórios sobre *docker* e *docker-compose* com teoria e prática. Todas as referências utilizadas estão disponíveis na seção *referências*.

Serão três partes subdivididas como segue: 

* parte 1
  * conceitos bases;
  * contexto;
  * instalação;
  * primeira execução;
  * imagem e contêiner;
  * básico sobre cliente docker, linha de comando;
  * executar e encerrar contêineres;
  * mergulhando nas imagens;
  * construindo imagens;
  * definindo condições iniciais do contêiner;
  * usando volume e portas para interagir com contêineres;
  * permitindo conexões externas com contêineres;
  * exercícios;
  * referências;
* parte 2
  * executar um grupo de contêineres que interajam entre si via http;
  * executar um grupo de contêineres que interajam entre si via volume;
  * escalar manualmente aplicações;
  * usar softwares de terceiros nos contêineres como parte de um projeto, tais como banco de dados;
* parte 3
  * examinar as imagens que são utilizadas nos contêineres;
  * utilizar métodos para reduzir o tamanho do contêiner e o tempo de construção, tal como *build* em vários estágios;
  * deploy automático de contêineres;

Esse material tem o caráter introdutório, com teoria e prática sobre os temas, abordando tópicos fundamentais sobre docker e conteinerização. Embora exista uma espécie de índice, os temas se desenvolvem sem um lugar específico, mas ao longo do texto todo.

Esta é a primeira parte.

## Primeira Parte

### Conceitos bases

Contêiner é uma tecnologia de virtualização do tipo de isolamento em nível de sistema operacional (os-level virtualization), diferenciando-se das virtualizações do tipo *hipervisionadas* (hardware hypervisors), cuja interação com o hardware é intermediada por meio de um software de supervisão. O isolamento de um contêiner ocorre no nível do sistema operacional por meio de técnicas de segregação de instâncias de espaços de usuários. 

*Chroot* é uma das primeiras técnicas, se não a primeira, de isolamento em nível de sistema operacional pela qual processos e seus subprocessos são isolados do resto do sistema de arquivos e processos. Essa técnica consiste em recriar toda a árvore de diretórios necessárias para executar um programa, copiando todos os programas do sistema necessários para executá-lo e ajustando as referências para esse novo simulacro. Em seguida, é utilizado o `chroot` para modificar o diretório root para o diretório base recriado, criando um isolamento para o programa executado que não conseguirá acessar nada além do diretório base.

Embora a técnica de *chroot* traga a ideia de isolamento em nível de sistema operacional, a técnica é muito limitada porque não faz isolamento no nível do espaço do kernel, mantendo todos os recursos de hardware compartilhados. Contêiner é uma técnica de isolamento em nível de sistema operacional que é implementada no nível do espaço do kernel, permitindo um isolamento total dos recursos de hardware. Dentre as vantagens desse tipo de técnica estão: reduzida sobrecarga, principalmente se comparado à virtualização hipersivionada, pois os programas que rodam dentro de uma partição virtual usam as interfaces de chamadas de sistema padrões do sistema operacional.

Com isso, *docker* é um conjunto de ferramentas cujo objetivo é implementar virtualização de nível do sistema operacional, com isolamento e mecanismos de gerenciamento de recursos, utilizando `cgroups` e `namespaces`. A base desse conjunto de ferramentas é composta por: servidor (*docker* *daemon*), comunicação por *rest* *api* (*docker* engine *api*), cliente (*docker* cli, *docker* desktop), repositório de imagens (*docker* hub). O serviço de *daemon* do *docker*, também chamado de *dockerd*, é um processo persistente que gerencia os contêineres e manipula objetos vinculados a eles. O *daemon* responde a requisições que chegam pela *api* e que se originam geralmente pela ferramenta de linha de comando. 

### Contexto

*Docker* é uma consolidada ferramenta quando se pensa em integração e entrega contínuas em ambientes de desenvolvimento de software, pois:

* permite ambientes isolados e customizados, o que significa instalar no contêiner apenas o que é necessário para execução do software principal, seja o próprio software ou algum servidor;
* permite um completo isolamento de tal modo que em um mesmo host pode-se ter vários contêineres executando diferentes versões de *python*, por exemplo, sem nenhuma interferência entre os próprios contêineres e entres eles e o host;
* permite a criação de imagens, com ambientes predefinidos, que servem de base para criação de outros contêineres, mantendo integridade entre eles. Além disso, suporta infraestrutura como código (*IaC*), *dockerfile*, no qual se especifica várias características para uma imagem que servirá de protótipo para criação de contêineres;
* em virtude da baixa sobrecarga, um contêiner deve ser efêmero, isto é, é possível criar várias instâncias de um mesmo contêiner, aumentando a disponibilidade de um serviço, mas também deve ser possível desalocar os contêineres, reduzindo a disponibilidade. O termo que descreve esse processo é orquestração, cujo objetivo é aumentar ou diminuir recursos (*horizontal scaling*);

### Instalação

Dois são os pré-requisitos de instalação: *docker* e *docker-compose*, ambos da *docker inc.*. *Docker* possui um rico ambiente de documentação que pode ser acessado [aqui](docs.docker.com){:target="_blank"}. Em se tratando de instalação, mais informações podem ser obtidas [aqui](docs.docker.com/engine/install/){:target="_blank"}. O mesmo vale para [docker-compose](docs.docker.com/compose/){:target="_blank"} e os documentos de [instalação](https://docs.docker.com/compose/install/){:target="_blank"} para ele.

Ademais, em se tratando de instalação do *docker* é importante saber que a arquitetura inicial exigia que ela fosse feita como super usuário (root / administrador). A partir da versão 19.03 essa exigência foi alterada, permitindo a instalação do *docker* com usuário comum. Tal assunto pode ser visto [aqui](https://docs.docker.com/engine/security/rootless/){:target="_blank"}.

Além disso, é comum que após a instalação do *docker*, no caso em que se opte por executá-lo como super usuário, o usuário responsável por executá-lo seja incluído no grupo `docker`. Assim, não será necessário prefaciar o comando `docker` com `sudo`. Mais detalhes [aqui](https://docs.docker.com/engine/install/linux-postinstall/){:target="_blank"}. 

### Primeira execução

A melhor forma para validar a instalação do *docker* é realizar uma primeira execução de uma imagem chamada *hello-world* usando o comando `docker container run hello-world`. Nesse caso, meu usuário foi incluído ao grupo `docker`, removendo a necessidade de prefaciar o comando com `sudo`.

~~~ shell
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
2db29710123e: Pull complete 
Digest: sha256:aa0cc8055b82dc2509bed2e19b275c8f463506616377219d9642221ab53cf9fe
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

#
# obtendo esse retorno, a instalação do *docker* foi um sucesso.
#
~~~

Esse é o retorno de uma execução que foi feita pela primeira vez com referência à imagem `hello-world`. As cinco primeiras linhas nos indicam isso porque nelas está registrado a tentativa do *docker* de buscar a imagem, que segue o seguinte procedimento:

1. primeiro busca-se a imagem no repositório local. No caso acima, não foi encontrada, `Unable to find image 'hello-world:latest' locally`;
2. segundo, caso não encontrando no primeiro passo, busca-se no repositório público.

O repositório oficial do *docker* encontra-se [aqui](hub.docker.com){:target="_blank"} e nele imagens oficiais de ferramentas, aplicações e sistemas operacionais são armazenadas e disponibilizadas para uso geral. Ademais, qualquer usuário pode incluir imagens no repositório, seguindo as políticas específicas de assinatura, e nesse sentido imagens não oficiais também estão disponíveis.

Imagens não oficinais são imagens que não tem uma garantia da sua finalidade, pois podem traz algum malware. Lembre-se que qualquer um pode disponibilizar imagens no repositório do *docker*, sem curadoria da *docker inc*, e o download é feito pela internet. Esses fatores colocam a exigência de garantir que o que está sendo executado é seguro.

Vinculada a imagem existem duas informações que precisam ser observadas. A *tag* é utilizada para indicar a versão da imagem e quando não informada é utilizada `latest`. Dependendo do gerenciamento da imagem, tags marcam ciclos de atualização distintos. Por exemplo, na imagem do *kalilinux* existe uma *tag* chamada *kali-rolling* que indica que a imagem é semanalmente atualizada com novas ferramentas e versões. E existe *kali-last-release* que segue uma atualização a cada quatro meses. *Digest* é outra informação importante, pois pode ser utilizado para validar se a imagem que chegou até o repositório local não foi adulterada no caminho. 

As sucessivas execuções do comando `docker container run hello-world` ou `docker container run hello-world:latest` encontrará a imagem no repositório local, sem mostrar no log as primeiras cinco linhas analisadas acima. Isso muda caso a imagem mude ou a *tag*.

~~~ shell
$ docker container run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
~~~

### Imagem e contêiner

Contêineres são instâncias de imagens, as quais, por sua vez, são criadas de acordo com a finalidade desejada para o contêiner, servindo como um protótipo. *Docker* possui uma funcionalidade para compilação de imagens a partir de arquivo de instruções, chamado de *dockerfile*. Entra em jogo o conceito de infraestrutura como código, pela qual a configuração da infraestrutura de ambientes é feita por meio de arquivos de instruções. 

Um arquivo `dockerfile` suporta uma série de instruções, que podem ser conferidas [aqui](https://docs.docker.com/engine/reference/builder/){:target="_blank"}, mas uma versão básica de um arquivo seria:

~~~ dockerfile
FROM <imagem>:<tag>

RUN <instala dependências>

CMD <comandos que são executados quando há uma solicitação de execução de contêiner, `docker container run`>
~~~

A partir de um `dockerfile`, pode-se executar o comando `docker image build`, o qual analisará o arquivo e compilará uma imagem. A partir dessa funcionalidade o *docker* tornou-se uma poderosa ferramenta de automação para criação de ambientes com grande relevância ao longo do processo de integração e entrega contínuos. 

O `dockerfile` e a funcionalidade de compilação de imagens do *docker* garantem que todas as imagens serão criadas com as mesmas características, as quais são registradas no arquivo de instruções, não importando o ambiente que hospede os contêineres. Isso permite com que a aplicação seja isolada desse ambiente com todas as suas dependências e integrada ao longo de todo o processo de desenvolvimento até a entrega do artefato final. Reduzindo, com isso, a famosa situação *na-minha-máquina-o-problema-não-acontece*.

Ainda sobre imagens, elas devem ser consideradas imutáveis e com algum grau de efemeridade. Recomenda-se que a cada modificação o arquivo de instruções seja modificado e a imagem recriada, evitando-se, assim, inconsistências.

Para listar as imagens no ambiente local, usa-se o seguinte comando `docker image ls` ou `docker images`. E como resultado, temos:

~~~ bash
~$ docker images
REPOSITORY                          TAG        IMAGE ID       CREATED          SIZE
hello-world                         latest     feb5d9fea6a5   16 months ago    13.3kB
~~~

Ao executar `docker container run hello-world`, o *docker* faz download da imagem para o repositório local, o qual será requisitado toda vez que um contêiner for executado com referência a `hello-world` ou `hello-world:latest`. Nos casos em que um volume grande de diversas imagens são utilizadas constantemente, é importante revisar o repositório local para controlar os recursos dispendidos em disco para isso.

Os contêineres são imutáveis e efêmeros, a criação e destruição deles não deve ser um problema. Por conta disso, alterações que devem persistir em todas as instâncias não devem ser feitas em um contêiner específico, mas no arquivo `dockerfile`, desde o qual gerou-se uma imagem que serve de base para os contêineres. Contêineres são ambientes isolados entre si e do ambiente que os hospeda, exceto quando definidos métodos de interação via stdin/stdout/stderr, volumes compartilhados, e TCP/UDP.

Para listar os contêineres em execução, utiliza-se `docker container ls` ou `docker ps`. E como resultado, pode-se receber uma lista vazio, quando não houver nenhum contêiner em execução.

~~~ bash
$ docker container ls
CONTAINER ID   IMAGE              COMMAND                  CREATED             STATUS             PORTS     NAMES
~~~

Mesmo que tenhamos executado o comando `docker container run hello-world`, a imagem em questão ao ser instanciada como contêiner atinge a sua finalidade, escrever na saída padrão a mensagem *hello-world*, e a sua execução é encerrada. Nesses casos, o ideal é executar `docker container ls -a` ou `docker ps -a`. Assim, os contêineres que já estão encerrados serão listados, conforme a seguir:

~~~ bash
$ docker ps -a
CONTAINER ID   IMAGE                 COMMAND           CREATED         STATUS                     PORTS     NAMES
6630496d6fe8   hello-world           "/hello"          4 seconds ago   Exited (0) 3 seconds ago             laughing_carver
~~~

### Básico sobre cliente docker, linha de comando

*Docker* funciona em uma arquitetura cliente-servidor: um cliente, geralmente linha de comando, solicita via *rest* *api* para o servidor, *daemon*, que execute algum comando. Embora existam outros clientes, a interface de linha de comando (cli - command line interface) é o cliente mais comum e versátil e uma lista completa das *flags* é encontrada [aqui](https://docs.docker.com/engine/reference/commandline/cli/){:target="_blank"}.

`docker container run <image>` é um comando que solicita ao *daemon* que crie um container, utilizando a imagem informada, e que esse contêiner seja executado. No exemplo que estamos utilizando até aqui, o contêiner é executado e se encerra porque o processo principal do contêiner executou a sua função. Além disso, a imagem também fica registrada no repositório local. Para removê-la, usa-se o seguinte comando `docker image rm hello-world`. No entanto, um contêiner ainda faz referência a essa imagem e por isso obtemos a seguinte mensagem `Error response from daemon: conflict: unable to remove repository reference "hello-world" (must force) - container <container ID> is using its referenced image <image ID>`.

Para listar os contêineres encerrados usa-se `docker container ls -a` ou `docker ps -a` ou ainda `docker container ls -a | grep hello-world`. Nesse última caso, `| grep hello-world` filtra o retorno pela palavra *hello-world*.

Sabendo as informações sobre o(s) contêiner(es) que são dependentes da imagem que queremos remover, basta executar `docker container rm <id>` ou uma sequência de ids `docker container rm <id> <id> <id> <id>`. Os ids utilizados pelo *docker* em vários contextos são hashes únicos. Para facilitar o uso deles, é possível utilizar os primeiros três ou mais caracteres de um id para referenciá-lo. Supondo que o id do contêiner a ser removido seja *6630496d6fe8*, bastaria `docker container rm 663`. Acaso *663* não identifique um único objeto, mais um dígito precisa ser incluído.

Como *docker* suporta centenas de contêines e pode ser que uma grande quantidade deles esteja parado, para excluí-los usa-se o commando `docker container prune`. `Prune` é uma *flag* versátil que pode ser utilizado em vários contextos para fazer uma *limpeza* de objetos que não estão em um status de execução: `docker image prune`, `docker system prune`.

Depois de remover o contêiner, será possível remover a image `hello-world`, e para confirmação utilize `docker image ls`. Imagens podem ser obtidas usando o comando `docker image pull hello-world`. E para consultar no repositório do *docker*, pode-se utilizar o comando `docker search hello-world`.

Agora, vamos buscar por uma imagem que execute a aplicação *nginx*. Para isso, comecemos pelo comando `docker search nginx` e como resultado

~~~ bash
NAME                         DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
nginx                        Official build of Nginx.                        17972     [OK]       
linuxserver/nginx            An Nginx container, brought to you by LinuxS…   183                  
bitnami/nginx                Bitnami nginx Docker Image                      150                  [OK]
ubuntu/nginx                 Nginx, a high-performance reverse proxy & we…   75                   
~~~

Os primeiros registro da busca indicam sempre as distribuições oficiais, que devem ter prioridade caso você não tenha confiança em algum fornecedor especifico além do oficial. Com isso, busca-se garantir integridade da imagem e dos contêineres que serão gerados, reduzindo o risco de gerar algum problema no host ou na aplicação dependentes do contêiner. Atente-se também ao indicar de reconhecimento da comunidade,*stars*, desde o qual pode-se considerar que uma imagem é segura. 

Escolhida a imagem, já sabemos como proceder: `docker container run ngix`. Essa é a maneira mais simples, embora nesse comando existam outros processos subjacentes, tais como: `docker pull ngix`, `docker create ngix`, `docker container start ngix`.

Note que ao executar `run nginx` o terminal congela logo após iniciar o contêiner. Isso é o comportamente padrão do *docker* em contêineres que executam tarefas persistentes, ou seja, o terminal fica vinculado a executação. Se executar o comando `docker ps` em outro terminal, o contêiner nginx estará em execução. Em alguns cenários, opta-se por executar o contêiner desvinculando-o do terminal. Para isso, usa-se o comando `docker container run -d nginx`. `-d` é a forma simplificada de `--deatach`, os quais modificam o comportamento do comando `docker run` para que o contêiner seja executado em *background* e que no terminal seja apenas impresso o *id* do contêiner.

Em resumo: contêineres que executam processos persistêntes podem ser colocados em *background*, desvinculando-o do terminal que o executou: `docker container run -d nginx`. Parar um contêiner: `docker container stop <id ou nome>`. Remover um contêiner: `docker container rm <id ou nome>`, e um imagem: `docker imagem rm nginx`.

Com o tempo é comum que o *docker* fique *entupido* de contêineres e imagens. Para resolver isso é importante fazer uma *limpeza* usando a *flag* `prune`: `docker container prune`, `docker image prune`. Ou ainda com a *flag* `--rm` junto ao comando `docker run`. Desse modo, *docker* removerá o contêiner logo em seguida ao fim de sua execução. Exemplo: `docker container run -it --rm ubuntu`.

### Executar e encerrar contêineres

Há diferentes *flags* que modificam o modo como um contêiner é executado e como interage com o terminal que o executou. A maneira mais simples para executar um contêiner é `docker run ubuntu`, embora nele estejam ocultos outros passos.

1. já sabemos que se a imagem não estiver disponível localmente, *docker* irá buscá-la no repositório. Para verificar se uma imagem existe localmente podemos usar o comando `docker images ubuntu`. Caso não entre, é possível localizar a imagem no repositório `docker search ubuntu`. 
2. Caso exista a imagem no repositório, `docker pull ubuntu` fará o trabalho de trazer a imagem para o repositório local.
3. Com imagem disponível localmente, `docker create ubuntu` irá criar um contêiner baseado na imagem, sem executá-lo. Nesse momento algumas *flags* podem ser usadas para modificar o comportamento do contêiner quando o mesmo for executado. Algumas delas discutiremos a seguir;
4. `docker start <id>` executa o contêiner.

`docker run ubuntu` abstrai todos esses passos e facilita a execução de contêineres. Ao executar esse comando, embora a imagem seja de um sistema operacional, o contêiner será encerrado logo após a inicialização. Talvez estivêssemos esperando um comportamente similar a uma máquina virtual, que permanece executando o sistema operacional mesmo que nenhum processo específico seja executado. Cabe destacar que contêineres diferem em vários aspectos de máquinas virtuais, inclusive nesse. Eles têm o propósito de isolar e executar processos específicos, incluindo aí tudo que é necessário para isso, e quando esse processo específico se encerra, junto com ele também o contêiner se encerrá.

No caso da imagem *ubuntu*, ela foi construída para executar um processo de *bash* quando inicializada sem um comando específico, o qual pode ser confirmado no registro do contêiner. 

~~~ shell
docker ps -a

CONTAINER ID   IMAGE              COMMAND                  CREATED         STATUS                     PORTS     NAMES
d1a6ef4dc22b   ubuntu             "bash"                   4 seconds ago   Exited (0) 3 seconds ago             eager_pike
~~~

O comando executado no contêiner foi *bash* e ele encerrou a execução com código *0*, o que indica sucesso. No entanto, o contêiner se encerrou e não permitiu nenhuma interação, como se nada tivesse acontecido.

~~~ shell
$ docker run ubuntu echo "echoing"
echoing

$ docker ps -a
CONTAINER ID   IMAGE                                                          COMMAND                  CREATED          STATUS                      PORTS     NAMES
0bd64f012925   ubuntu                                                         "echo echoing"           14 seconds ago   Exited (0) 13 seconds ago
~~~

Nessa outra tentativa, obtivemos uma interação. O contêiner se encerra imprimindo no console a palavra *echoing*, ou seja, o comando `echo echoing` foi exeutado. Por padrão, `docker run` não conecta o *stream* padrão de entrada (*sdtin*) do contêiner com o terminal, mas conecta o *stream* padrão de saída (*stdout*) e o *stream* padrão de erro (*sdterr*). Por isso recebemos o resultado da execução do comando. Nesse cenário, em casos em que o processo exige interação de entrada, o processo é encerrado imediatamente. Como foi o caso quando executado `bash`. Ou ainda no próximo caso.

~~~ shell
$ docker run ubuntu passwd root
New password: Password change has been aborted.
passwd: Authentication token manipulation error
passwd: password unchanged
~~~

O comando `passwd` exige interação de entrada com o usuário. Como nada foi vinculado à entrada padrão, o comando encerra com erro. Para vincular o *stream* padrão de entrada do contêiner com o terminal, usa-se a *flag* `-i`.

~~~ shell
$ docker run -i ubuntu passwd root
New password: root
Retype new password: root
passwd: password updated successfully
~~~

Mas nem sempre a entrada padrão virá de um terminal. Pode, por exemplo, ser vinculado pelo *pipe* de um retorno do comando `echo "This is a piped input"`, como no exemplo a seguir.

~~~ shell
$ echo "This is a piped input" | docker run -i ubuntu cat
This is a piped input
~~~

Ou ainda nesse outro caso, no qual a entrada padrão do contêiner é vinculado ao terminal e o comando passado como parâmetro o recebe e executa. A primeira linha é entrada de dados, a segunda, o retorno do comando `cat`.

~~~ shell
$ docker run -i ubuntu cat
docker test
docker test
test docker
test docker
~~~

Nesse último caso, a entrada padrão do contêiner foi vinculado ao terminal que executou o processo. A primeira linha *docker test* foi digitada, a segunda, foi resultado do comando `cat`. Um último exemplo:

~~~ shell
$ docker run -i ubuntu rev
1234567890
0987654321
~~~

Embora nesses exemplos tenhamos conseguido interagir com comandos executados dentro do contêiner, enviando e recebendo dados, em certa situações é necessário interagir com um terminal dentro do contêiner. Nesse caso, é necessário alocar um *pseudo-TTY*, ou seja, um terminal emulado. Para isso, a *flag* utilizada é `--tty` ou `-t`.

~~~ shell
$ docker run -t ubuntu
root@c197f5486a62:/# 
~~~

No exemplo acima, foi requisitado um terminal para o *docker* e nos foi entregue `root@c197f5486a62`. No entanto, nenhum *stream* de entrada foi vinculado ao *stream* de entrada padrão do contêiner. Isso fica mais explicito quando vinculamos um *stream* por *pipe*.

~~~ shell
$ echo "from echo" | docker run -t ubuntu cat

~~~

O retorno é uma linha em branco. Embora o cat tenha se conectado ao terminal emulado, o terminal emulado não se conectou à entrada do *pipe* porque não foi designado ao *docker* que ele exposse o *stream* de entrada padrão para receber entrada de dados externo. Lembremos que um contêiner é totalmente isolado, exceto se configurado para se conectar com o *mundo exterior*, e exceto pelo sdtout e sdterr.

Com isso, no geral e na maioria das vezes é interessante criar o contêiner com `-it`, `docker create -it ubuntu` ou `docker run -it ubuntu`. Caso se opte por não usar `docker run`, quando o contêiner for executado, deve-se vincular o terminal ao contêiner `docker start -ai <id>`. 

Apenas como registro, `-it` é a forma simplificada para `--interactive` e `--tty`. 

`docker run -d -it --name looper ubuntu sh -c 'while true; do date; sleep 1; done'` é um comando que traz uma complexidade maior, embora conheçamos algumas das *flags*; `-d`, `-it`; e parâmetros `sh -c 'while true; do date; sleep 1; done'`. 

* `-d` executa o contêiner em *background*;
* `-it` cria um terminal e abre o *stream* padrão de entrada do contêiner que poderá ser vinculado ao terminal;
* `--name` cria o contêiner com um nome inicial, quando não informado a *docker* engine gera um nome aleatório, que pode ser utilizado no lugar do id;
* `sh -c 'while true; do date; sleep 1; done'` comando que será executado no shell dentro do contêiner. O comando executa uma repetição infinita, `while true`, que imprime a data e a hora, `do date`, em intervalos de um segundo, `sleep 1`;

~~~ shell
$ docker run -d -it --name looper ubuntu sh -c 'while true; do date; sleep 1; done'
881a12f8713635d23166730a9bbffbb6d4fe5fbeb20fad5f578c6c1cd2f4a2a6
~~~

O retorno do comando é, como esperado, o hash de identificação. Isso porque utilizamos a *flag* `-d`. Para verificar se o contêiner está em execução `docker container ls` ou `docker ps`. E ainda podemos acessar o que o contêiner está enviando via *stdout*.

~~~ shell
$ docker logs -f looper
Wed Jan 25 01:20:10 UTC 2023
Wed Jan 25 01:20:11 UTC 2023
Wed Jan 25 01:20:12 UTC 2023
Wed Jan 25 01:20:13 UTC 2023
Wed Jan 25 01:20:14 UTC 2023
Wed Jan 25 01:20:15 UTC 2023
~~~

Em outro terminal é possível pausar o contêiner `docker pause looper` e o registro de log irá pausar também. Para retornar `docker unpause looper`.

~~~ shell
$ docker attach looper
Wed Jan 25 01:28:26 UTC 2023
Wed Jan 25 01:28:27 UTC 2023
Wed Jan 25 01:28:28 UTC 2023
Wed Jan 25 01:28:29 UTC 2023
Wed Jan 25 01:28:30 UTC 2023
Wed Jan 25 01:28:31 UTC 2023
~~~

Nesse último caso, usando o comando `attach` é possível conectar diretamente ao terminal dentro do *docker*, que no nosso exemplo está executando o comando `'while true; do date; sleep 1; done'` e imprimindo no terminal. Caso o comando seja encerrado, por exemplo `ctrl + c`, o contêiner se encerrará.

Para evitar com que contêineres sejam encerrados a partir de outros terminais, é possível utilizar a *flag* `--no-stdin` que não recebe entrada de dados.

~~~ shell
$ docker ps
CONTAINER ID   IMAGE                      COMMAND                  CREATED        STATUS       PORTS     NAMES

nonada@nonada:~$ docker start looper
looper
nonada@nonada:~$ docker attach --no-stdin looper
Wed Jan 25 01:35:30 UTC 2023
Wed Jan 25 01:35:31 UTC 2023
Wed Jan 25 01:35:32 UTC 2023
Wed Jan 25 01:35:33 UTC 2023
Wed Jan 25 01:35:34 UTC 2023
^C
nonada@nonada:~$ docker ps
CONTAINER ID   IMAGE                      COMMAND                  CREATED          STATUS          PORTS     NAMES
881a12f87136   ubuntu                     "sh -c 'while true; …"   15 minutes ago   Up 22 seconds             looper
~~~

O contêiner continua em execução, `ctrl + c` apenas desconecta do *sdtout*.

Em um contêiner em execução, podemos criar um novo processo de terminal e acessá-lo.

~~~
docker exec -it looper bash
root@df9319bd49cb:/#
root@df9319bd49cb:/# ps aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.1  0.0   2888   988 pts/0    Ss+  10:49   0:00 sh -c while true; do date; sleep 1; done
root          53  0.2  0.0   4624  3516 pts/1    Ss   10:49   0:00 bash
root          86  0.0  0.0   2788  1032 pts/0    S+   10:49   0:00 sleep 1
root          87  0.0  0.0   7060  1576 pts/1    R+   10:49   0:00 ps aux
~~~

Com `ps aux` notamos que nosso processo inicial está em execução.

Algumas vezes usar `docker stop` pode ser lento, pois ele segue um tempo de espera para garantir que todos as aplicações se encerrem depois de um `SIGTERM` com `SIGKILL`. Caso seja preciso encerrar um contêiner mais rapidamente, há algumas opções.

~~~ shell
docker kill looper
docker rm looper

docker run -d --rm -it --name looper-it ubuntu sh -c 'while true; do date; sleep 1; done'
~~~

As duas primeiras linhas encerar a execução de um contêiner e removê-lo. A terceira é uma maneira de criar um contêiner de tal modo que ao encerrar o processo principal, o contêiner é removido.

### Mergulhando nas imagens

Imagens são protótipos básicos para construção de outras imagens ou contêineres. Há dois lugares básico de armazenamento de imagens: o primeiro é local, o segundo é o repositório público do *docker*. E como já vimos é possível localizar imagens no repositório público pelo comando `docker search postgres`.

~~~ shell
$ docker search postgres
NAME                               DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
postgres                           The PostgreSQL object-relational database sy…   11897     [OK]       
bitnami/postgresql                 Bitnami PostgreSQL Docker Image                 178                  [OK]
circleci/postgres                  The PostgreSQL object-relational database sy…   30                   
ubuntu/postgres                    PostgreSQL is an open source object-relation…   23                   
bitnami/postgresql-repmgr                                                          19                   
rapidfort/postgresql               RapidFort optimized, hardened image for Post…   16                   
~~~

As imagens que são oficiais, além da manutenção dos autores da imagem, são também cuidadas e revistas pela *docker inc.*. Essas imagens oficiais seguem um padrão de criação que visa disponibilizar uma base de sistema operacional essencial que sirva de começo para a maioria dos usuários; disponibilizar imagens de ambientes de linguagens de programação populares, bancos de dados e outros serviços de modo similar ao que os serviços PaaS oferecem; servem de exemplo das melhores práticas para `dockerfile`; garantem que atualizações de segurança sejam aplicadas em tempo hábil.

Além do *ok* na coluna *official*, imagens oficiais não possuem o prefixo da organização. Já *bitnami/postgresql* não é uma imagem oficial, o nome está com o prefixo da organização, mas a coluna *automated* está como ok, o que indica que essa imagem é construída diretamente do repositório fonte.

*Docker hub* é o repositório oficial do *docker* para imagens oficiais e não-oficiais. No entanto, ele não é o único. Atualmente existe *quay.io*, vinculado à *red hat*, que disponibiliza imagens para *docker*, *podman* e *rkt*. Embora não se possa utilizar `docker search` é possível indicar um endereço para o contêiner `docker pull quay.io/nordstrom/hello-world:2.0`.

Todas as imagens possuem uma *tag* como, por exemplo, *latest*, que indica que é a última versão contruída e enviada ao repositório. Mas cada mantenedor estabele seus próprios critérios em relação as *tags* e o que elas indicam. Por exemplo, a imagem *ubuntu*, além de *latest*, possui uma série de outras tags, dentre elas, algumas que indicam versões específicas, tais como `ubuntu:23.04`, `ubuntu:22.10`, `ubuntu:22.04`, `ubuntu:18.04`.

~~~ shell
$ docker pull ubuntu:23.04
23.04: Pulling from library/ubuntu
2627e5235478: Pull complete 
Digest: sha256:2ca8fe42bcc2979f66dd80c2987a43cfc5502626094b7f838f89759173f3956b
Status: Downloaded newer image for ubuntu:23.04
docker.io/library/ubuntu:23.04
~~~

Imagens são construídas por diferentes camadas e metadados. Em alguns circunstâncias as camadas podem otimizar o processo de obtenção de uma outra imagem, quando essa compartilha uma camada base que já está disponível localmente. Geralmente essa camada base é outra imagem. Imagens permitem tags e, consequentemente, permitem serem entiquetas usando o comando `docker tag`. O nome de uma imagem é composto de três partes: *repositório/organização/imagem:tag*. As imagens que são oficinais, são indicadas apenas por *imagem:tag*.

### Construindo imagens

Uma das grandes potencialidades do *docker* é permitir a criação de imagens customizadas com o arquivo de instruções `dockerfile`. Sigamos um experimente sobre isso.

~~~ shell
$ touch docker-custom.sh
$ ls docker-custom.sh 
docker-custom.sh
$ echo \#\!/bin/sh > docker-custom.sh
$ echo 'echo "Hello, docker!"' >> docker-custom.sh
$ cat docker-custom.sh
#!/bin/bash
echo "Hello, docker!"
$ 
~~~

Testar `docker-custom.sh`.

~~~ shell
$ chmod +x docker-custom.sh
$ ./docker-custom.sh 
Hello, docker!
$ 
~~~

Nossa imagem irá executar o programa `docker-custom.sh`, mas para isso ela precisa de uma base que execute arquivos `.sh`. Geralmente, uma imagem se baseia em outra que tem uma versão *enxuta* do linux, a partir da qual outras ferramentas são instaladas para atingir o propósito ao qual os contêineres gerados dela se destinam. *Dockerfile* é um arquivo texto, então podemos usar editor de texto para escrevê-lo.

~~~ shell
$ touch Dockerfile
$ echo "# Start from the alpine image that is smaller but no fancy tools"
$ echo "FROM alpine:3.13" >> Dockerfile
$ echo >> Dockerfile
$ echo "# Use /usr/src/app as our workdir. The following instructions will be executed in this location." >> Dockerfile
$ echo "WORKDIR /usr/src/app" >> Dockerfile
$ echo >> Dockerfile
$ echo "# Copy the docker-custom.sh file from this location to /usr/src/app/ creating /usr/src/app/docker-custom.sh" >> Dockerfile
$ echo "COPY docker-custom.sh ." >> Dockerfile
$ echo >> Dockerfile
$ echo "# Alternatively, if we skipped chmod earlier, we can add execution permissions during the build." >> Dockerfile
$ echo "# RUN chmod +x docker-custom.sh" >> Dockerfile
$ echo >> Dockerfile
$ echo "# When running docker run the command will be ./docker-custom.sh" >> Dockerfile
$ echo "CMD ./docker-custom.sh" >> Dockerfile
$ echo >> Dockerfile
$ cat Dockerfile
# Start from the alpine image that is smaller but no fancy tools
FROM alpine:3.13

# Use /usr/src/app as our workdir. The following instructions will be executed in this location.
WORKDIR /usr/src/app

# Copy the docker-custom.sh file from this location to /usr/src/app/ creating /usr/src/app/docker-custom.sh
COPY docker-custom.sh .

# Alternatively, if we skipped chmod earlier, we can add execution permissions during the build.
# RUN chmod +x docker-custom.sh

# When running docker run the command will be ./docker-custom.sh
CMD ./docker-custom.sh
~~~

Então, vamos compilar uma imagem usando `docker build`, onde `-t hello-docker-custom` será o nome da imagem, o qual poderia ser acrescido de uma outra *tag* tal como `-t hello-docker-custom:v1` ou `-t hello-docker-custom:latest`. `.` designa o local do arquivo `dokerfile`.

~~~ shell
$ docker build . -t hello-docker-custom
Sending build context to Docker daemon  3.584kB
Step 1/4 : FROM alpine:3.13
3.13: Pulling from library/alpine
72cfd02ff4d0: Pull complete 
Digest: sha256:469b6e04ee185740477efa44ed5bdd64a07bbdd6c7e5f5d169e540889597b911
Status: Downloaded newer image for alpine:3.13
 ---> 6b5c5e00213a
Step 2/4 : WORKDIR /usr/src/app
 ---> Running in d40df5c012b6
Removing intermediate container d40df5c012b6
 ---> a7e261938b8b
Step 3/4 : COPY docker-custom.sh .
 ---> 4fdfefd0f0f4
Step 4/4 : CMD ./docker-custom.sh
 ---> Running in aca5cee54539
Removing intermediate container aca5cee54539
 ---> 0b58abcb10ac
Successfully built 0b58abcb10ac
Successfully tagged hello-docker-custom:latest
$ 
$ docker images
REPOSITORY                                                     TAG        IMAGE ID       CREATED         SIZE
hello-docker-custom                                            latest     0b58abcb10ac   6 minutes ago   5.62MB
alpine                                                         3.13       6b5c5e00213a   5 months ago    5.62MB
$
~~~

Finalizada a execução, podemos testar.

~~~ shell
$ docker run hello-docker-custom
Hello, docker!
~~~

Na construção da imagem acima podemos notar as camadas que a compõem. As camadas tem multiplas funções. Por um lado, pode ser interessante limitar o número de camadas para reduzir o espaço gasto com armazenamento, porém cada camada funciona como cache durante a construção de imagens. Caso apenas a última linha do *dockerfile* seja editada, o comando que faz a construção da imagem inicia a partir da camada editada e pula as camadas das seções anteriores. Isso permite construir imagens que podem ser usadas em *pipelines* mais rápidos.

Há duas formas de modificarmos a imagem `hello-docker-custom`. Uma das opções é modificar o arquivo de instruções, incluindo nele as modificações desejadas. A segunda opção é modificar um contêiner gerado pela imagem e criar uma imagem a partir do contêiner modificado. A primeira opção é considerada a prática mais sustentável e adequada para automação de contêineres, pois o arquivo de intruções é de fácil análise e checagem de integridade. Não obstante, sigamos com um exemplo do segundo cenário.

O objetivo desse exemplo é incluir um arquivo no contêiner executado a partir da imagem `hello-docker-custom` e, em seguida, gerar uma imagem a partir do contêiner modificado.

1. acessar o shell de uma instância de contêiner porque a nossa imagem não tem um processo constante.
~~~ shell
$ docker run -it hello-docker-custom sh
~~~

2. em outro terminal, criar o arquivo que queremos copiar para o contêiner.
~~~ shell
$ touch additional-file
$ echo "test file new image" > additional-file
~~~

3. copiar o arquivo usando o comando de cópia do *docker*, pelo qual é possível copias arquivos externos diretamente para dentro do contêiner.
~~~ shell
$ docker ps
CONTAINER ID   IMAGE                        COMMAND                  CREATED         STATUS             PORTS     NAMES
7d00fcbd2b12   hello-docker-custom          "sh"                     7 minutes ago   Up 7 minutes                 sharp_northcutt


$ docker cp ./additional-file sharp_northcutt:/usr/src/app/
~~~

4. confirmar a cópia no terminal do passo 1.
~~~ shell
/usr/src/app # ls -l
total 8
-rw-rw-r--    1 1001     1001            20 Jan 25 21:44 additional-file
-rwxrwxr-x    1 root     root            32 Jan 25 14:10 docker-custom.sh
/usr/src/app # 
~~~

5. listar as modificações em um contêiner comparando com a imagem base usando o comando `docker diff`. Nesse exemplo estamos usando as três primeiras letras do id. O comando retorna uma lista, identificando as mudanças com as letras A = adicionado, D = deletado, C = modificado. `A /root/.ash_history` é o arquivo de histórico criado pelo shell, e como consequência o diretório `C /root` aparece como modificado. O mesmo vale para o arquivo adicionado `A /usr/src/app/additional-file`, desde o qual os subdiretórios também aparecem como modificados.
~~~ shell
$ docker diff 7d0
C /root
A /root/.ash_history
C /usr
C /usr/src
C /usr/src/app
A /usr/src/app/additional-file
~~~

6. validar as alterações gerando uma nova imagem.
~~~ shell
$ docker commit sharp_northcutt hello-docker-custom-add-file
$ docker images
REPOSITORY                                                     TAG        IMAGE ID       CREATED         SIZE
hello-docker-custom-add-file                                   latest     2b46f876538d   6 seconds ago   5.62MB
hello-docker-custom                                            latest     3c67903d7912   8 hours ago     5.62MB
~~~

No entanto, como mencionado, essa técnica é a menos adequada para manter a confiabilidade e integridade das imagens e dos contêineres em um ambiente de integração e entregas contínuos. O melhor mesmo é introduzir a alteração no arquivo de intruções e compilar uma nova versão da imagem.

~~~ shell
$ touch Dockerfile
$ echo "# Start from the alpine image that is smaller but no fancy tools"
$ echo "FROM alpine:3.13" >> Dockerfile
$ echo >> Dockerfile
$ echo "# Use /usr/src/app as our workdir. The following instructions will be executed in this location." >> Dockerfile
$ echo "WORKDIR /usr/src/app" >> Dockerfile
$ echo >> Dockerfile
$ echo "# Copy the docker-custom.sh file from this location to /usr/src/app/ creating /usr/src/app/docker-custom.sh" >> Dockerfile
$ echo "COPY docker-custom.sh ." >> Dockerfile
$ echo >> Dockerfile
$ echo "# Copy the additional-file file from this location to /usr/src/app/ creating /usr/src/app/additional-file" >> Dockerfile
$ echo "COPY additional-file ." >> Dockerfile
$ echo >> Dockerfile
$ echo "# Alternatively, if we skipped chmod earlier, we can add execution permissions during the build." >> Dockerfile
$ echo "# RUN chmod +x docker-custom.sh" >> Dockerfile
$ echo >> Dockerfile
$ echo "# When running docker run the command will be ./docker-custom.sh" >> Dockerfile
$ echo "CMD ./docker-custom.sh" >> Dockerfile
$ echo >> Dockerfile
$ cat Dockerfile
# Start from the alpine image that is smaller but no fancy tools
FROM alpine:3.13

# Use /usr/src/app as our workdir. The following instructions will be executed in this location.
WORKDIR /usr/src/app

# Copy the docker-custom.sh file from this location to /usr/src/app/ creating /usr/src/app/docker-custom.sh
COPY docker-custom.sh .

# Copy the additional-file file from this location to /usr/src/app/ creating /usr/src/app/additional-file
COPY additional-file .

# Alternatively, if we skipped chmod earlier, we can add execution permissions during the build.
# RUN chmod +x docker-custom.sh

# When running docker run the command will be ./docker-custom.sh
CMD ./docker-custom.sh

$ docker build . -t hello-docker-custom:v2
Sending build context to Docker daemon  4.608kB
Step 1/5 : FROM alpine:3.13
 ---> 6b5c5e00213a
Step 2/5 : WORKDIR /usr/src/app
 ---> Using cache
 ---> 9eb188839ca5
Step 3/5 : COPY docker-custom.sh .
 ---> Using cache
 ---> 3f993b722931
Step 4/5 : COPY additional-file .
 ---> 6c33f7f73477
Step 5/5 : CMD ./docker-custom.sh
 ---> Running in 472ff4ec3b7b
Removing intermediate container 472ff4ec3b7b
 ---> 79bd587fa113
Successfully built 79bd587fa113
Successfully tagged hello-docker-custom:v2
$ docker images
REPOSITORY                                                     TAG        IMAGE ID       CREATED         SIZE
hello-docker-custom                                            v2         79bd587fa113   6 seconds ago   5.62MB
hello-docker-custom-add-file                                   latest     2b46f876538d   9 minutes ago   5.62MB
hello-docker-custom                                            latest     3c67903d7912   8 hours ago     5.62MB
$ docker run -it hello-docker-custom:v2 sh
/usr/src/app # ls -l
total 8
-rw-rw-r--    1 root     root            20 Jan 25 22:11 additional-file
-rwxrwxr-x    1 root     root            32 Jan 25 14:10 docker-custom.sh
/usr/src/app # 
~~~

### Definindo condições iniciais do contêiner

Nesta seção iniciaremos por configurar um contêiner de modo interativo, ou seja, a partir de um contêiner criado sobre uma imagem base de um sistema operacional, realizaremos as instalação necessárias para execução da aplicação desejada. A imagem que buscamos deverá conseguir executar a aplicação *youtube-dl*, uma ferramenta de linha de comando que faz download de vídeos. Segundo [a documentação](https://ytdl-org.github.io/youtube-dl/download.html){:target="_blank"}, a única dependência é python em uma das seguintes versões 2.6, 2.7, 3.2+.

1. primeiro passo é executar um contêiner com ubuntu:18.04.
~~~ shell
$ docker run -it ubuntu:18.04
Unable to find image 'ubuntu:18.04' locally
18.04: Pulling from library/ubuntu
a055bf07b5b0: Pull complete 
Digest: sha256:c1d0baf2425ecef88a2f0c3543ec43690dc16cc80d3c4e593bb95e4f45390e45
Status: Downloaded newer image for ubuntu:18.04
root@2b927087df24:/#
~~~
2. estamos com um contêiner executando ubuntu. No entanto, as imagens de sistemas operacionais são geralmente *enxutas*, com poucas aplicações, inclusive as mais básicas, garantindo com que no contêiner esteja instalado apenas o que é necessário para o seu propósito. Uma das ferramentas que precisaremos é o `curl`, utilizado basicamente para transferir dados através de vários protocolos, e dentre esses *http*. Precisaremos dele para que na hora de montar o contêiner a aplicação *youtube-dl* seja obtida do seu endereço web oficial.
~~~ shell
root@2b927087df24:/# curl --help
bash: curl: command not found
#
apt update && apt install -y curl
root@2b927087df24:/# curl --help
Usage: curl [options...] <url>
~~~
3. instalação da aplicação.
~~~ shell
root@2b927087df24:/# curl -L https://yt-dl.org/downloads/latest/youtube-dl -o /usr/local/bin/youtube-dl
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100     3  100     3    0     0      1      0  0:00:03  0:00:02  0:00:01     1
100     3    0     3    0     0      0      0 --:--:--  0:00:03 --:--:--     0
  0     0    0     0    0     0      0      0 --:--:--  0:00:03 --:--:--     0
100 1794k  100 1794k    0     0   375k      0  0:00:04  0:00:04 --:--:-- 3444k
#
root@2b927087df24:/# chmod a+rx /usr/local/bin/youtube-dl
root@2b927087df24:/# youtube-dl 
/usr/bin/env: 'python': No such file or directory
~~~
4. embora a aplicação esteja instalada, com permissões atribuidas, ela ainda necessita do python instalado.
~~~ shell
root@2b927087df24:/# python --version
bash: python: command not found
#
root@2b927087df24:/# apt install -y python
root@2b927087df24:/# python --version
Python 2.7.17
~~~
5. agora nossa aplicação pode executar. Mas, na sua primeira execução, somo alertamos sobre a variável de ambiente LC_ALL, cuja função é não restringir o nome dos arquivos por caracteres inválidos.
~~~ shell
root@2b927087df24:/# youtube-dl
WARNING: Assuming --restrict-filenames since file system encoding cannot encode all characters. Set the LC_ALL environment variable to fix this.
Usage: youtube-dl [OPTIONS] URL [URL...]
#
youtube-dl: error: You must provide at least one URL.
Type youtube-dl --help to see a list of all options.
#
root@2b927087df24:/# export LC_ALL=C.UTF-8
root@2b927087df24:/# echo $LC_ALL
C.UTF-8
~~~
6. por fim, a aplicação está rodando no contêiner.
~~~ shell
root@2b927087df24:/# youtube-dl https://imgur.com/JY5tHqr
[Imgur] JY5tHqr: Downloading webpage
[download] Destination: Imgur-JY5tHqr.mp4
[download] 100% of 190.20KiB in 00:01
root@2b927087df24:/# 
~~~

Estabelecido um ambiente mínimo para execução da aplicação, o recomendado é gerar um arquivo de instruções base para geração de uma imagem.

~~~ dockerfile
FROM ubuntu:18.04

WORKDIR /mydir

RUN apt-get update && apt-get install -y curl python
RUN curl -L https://yt-dl.org/downloads/latest/youtube-dl -o /usr/local/bin/youtube-dl
RUN chmod a+x /usr/local/bin/youtube-dl

ENV LC_ALL=C.UTF-8

#CMD ["/usr/local/bin/youtube-dl"]
ENTRYPOINT ["/usr/local/bin/youtube-dl"]
~~~

Todo arquivo `dockerfile` deve iniciar com a instrução `FROM`, exceto pelo uso da instrução `ARG`. `FROM` marca um novo estágio da criação da imagem que serve de base para todas as instruções subsequentes. A referência a imagem base, por exemplo, *ubuntu*, pode receber uma *tag*, *18.04*, ou um *digest*. `AS <name>` pode ser usado para nomear o novo estágio da criação da imagem.

A partir do momento em que a instrução `WORKDIR` é atribuída, as instruções `RUN`, `CMD`, `ENTRYPOINT`, `COPY`, `ADD` usam como referência o diretório atribuído a ela. Caso não seja especificado `WORKDIR` será `/`.

`RUN` executará qualquer comando em uma nova camada sobre a imagem atual e registrará os resultados como uma nova imagem base que servirá de base para as próximas instruções do `dockerfile`. Nesse sentido, `RUN` é uma instrução que executa em tempo de contrução das imagens, o que é ideal, por exemplo, para incluir pacotes em uma imagem. Há duas formas de se usar a instrução: `RUN <command>`, conhecida como formato *shell*, ou RUN `["executable", "param1", "param2"]`, conhecida como formato *exec*. Esta última exige que se use aspas duplas ao invés de simples, e que se use caracteres de escape como, por exemplo, `RUN ["c:\\windows\\system32\\tasklist.exe"]`. Essas exigências surgem porque no formato *exec* os valores são colocados em um json e tratados. No caso for formato *shell* é possível utilizar `\` para quebra de linhas, tal como no exemplo abaixo.

~~~ shell
RUN /bin/bash -c 'source $HOME/.bashrc; \
echo $HOME'
~~~

`ENV` é uma instrução vinculada a variáveis de ambiente e que uma vez utilizada disponibiliza a variável de ambiente para as instruções subsequentes, mas também para o contêiner, em tempo de execução. Há duas maneiras de utilizar a instrução. A primeira é chamada de estática porque fixa o valor no próprio arquivo de instruções, tal como no exemplo `ENV LC_ALL=C.UTF-8`. A segunda maneira é dinâmina, pois permite receber um valor pelo comando que constrói a imagem, tal como no exemplo:

~~~ dockerfile
ARG username
ENV env_username $username
~~~

Na versão simplificada de um arquivo de instruções, usamos a instrução `ARG` para receber um argumento da chamada do comando `docker build` e em seguida ele é atribuído a uma variável de ambiente, que poderá ser utilizada nas próximas instruções ou no contêiner em execução. A chamada do `docker build` ficaria assim: `docker build -t exemplo-env-dinamica --build-arg username=docker-user .` .

Em geral pode-se utilizar quantas instruções `ENV` forem necessárias, embora é preciso avaliar que em cada chamada da instrução uma nova camada intermediária será criada.

Por fim, comentarei sobre as duas últimas instruções, entre elas, uma está comentada, elas são: `CMD` e `ENTRYPOINT`. Ambas as instruções permitem definir comandos que serão executados quando o contêiner for executado. `ENTRYPOINT` define um executável principal que poderá ser combinado com argumentos recebidos por parâmetro no momento da execução do contêiner, tal como `$ docker run youtube-dl https://imgur.com/JY5tHqr`.

Por padrão, quando não atribuído um valor específico para `ENTRYPOINT` ele recebe `/bin/sh`. Por isso quando passamos um comando na execução de um contêiner, o comando é executado.

~~~ shell
$ docker run -it ubuntu ls
bin   dev  home  lib32	libx32	mnt  proc  run	 srv  tmp  var
boot  etc  lib	 lib64	media	opt  root  sbin  sys  usr
~~~

Ou ainda quando apenas a instrução `CMD` é utilizada com um caminho para um script. Nesse caso, o script é passado como argumento para `/bin/sh`.

|dockerfile|comando resultado|
|----------|-----------------|
|`ENTRYPOINT /bin/ping -c 3` e `CMD localhost` | `/bin/sh -c '/bin/ping -c 3' /bin/sh -c localhost`|
|`ENTRYPOINT ["/bin/ping","-c","3"]` e `CMD localhost` | `/bin/ping -c 3 /bin/sh -c localhost`|
|`ENTRYPOINT /bin/ping -c 3` e `CMD ["localhost"]` | 	`/bin/sh -c '/bin/ping -c 3' localhost` |
|`ENTRYPOINT ["/bin/ping","-c","3"]` e `CMD ["localhost"]` | `/bin/ping -c 3 localhost` |

`ENTRYPOINT` terá sempre um valor fixo nos contêineres executados a partir de uma mesma imagem. No entanto, em cada execução `docker run` é possível passar diferentes argumentos que funcionam como `CMD`, mesmo que ele esteja atribuído no dockerfile, o parâmetro passado na chamada do `docker run` o sobreescreverá. 

### Usando volume e portas para interagir com contêineres

Embora um contêiner seja uma unidade isolada, existe uma série de maneiras de interagir com ele. Nesta seção analizaremos duas maneiras: volume e portas.

A imagem que criamos na seção anterior tem como finalidade isolar uma aplicação de download de videos. Seria interessante, então, que o usuário no host conseguisse acessar os vídeos de modo simples. Uma forma de viabilizar isso é ligar um volume externo com um volume interno.

A técnica é chamada de *bind mount* e é feita pelo comando *docker run*: `docker run -v "$(pwd):/mydir" youtube-dl https://imgur.com/JY5tHqr`. Ademais, esssa ténica pode ser usada para conectar contêineres que precisam compartilhar arquivos entre si.

Outra estratégia é compartilhar um arquivo específico, por exemplo, um arquivo de configurações: `-v $(pwd)/config.file:/mydir/config.file`. Com isso, modificações no arquivo a partir do host refletem no contêiner.

### Permitindo conexões externas com contêineres

A segunda técnica é chamada de *port bind* e é muito utilizada para disponibilizar serviços que estão executando no contêiner para acesso externo, enviando ou recebendo mensagens para endereços *url* tal como `http://127.0.0.1:4000`. Isso é possível através de um mapeamento entre uma porta do computador host e uma porta do contêiner. A abertura de uma porta do lado de fora para o contêiner acontece em dois passos:

1. exposição de porta (exposing port);
2. publicação de porta (publishing port).

Exposição de porta indica que o contêiner passa a *escutar* em uma determinada porta e para isso utilize a instrução `EXPOSE <porta>` no `dockerfile`. Publicar uma porta indica ao *docker* para mapear portas do host para as portas do contêiner e é feito ao executar o contêiner: `-p <host-porta>:<container-porta>`.

Também é possível limitar as conexões para certo protocolo: `EXPOSE <porta>/udp` e `-p <host-porta>:<container-porta>/udp`.

Expor portas é uma questão crítica na segurança de ambientes e o modo como isso é feito pode ocasionar que qualquer um tenha acesso à porta exposta. Uma forma de reduzir a área de exposição é forçar que a ligação seja feita apenas com um host específico, tal como: `-p 127.0.0.1:33456:4000`. Assim, apenas conexões do localhost podem ser feitas. Menos recomendado é `-p 3456:3000`, que é igual a `-p 0.0.0.0:33456:4000`, significando que a porta `4000` está aberta para qualquer um.

### Exercícios

Todo: exercícios com foco nos temas acima.

## Referências

[hub.docker.com](hub.docker.com){:target="_blank"}.

[docs.docker.com](docs.docker.com){:target="_blank"}.

[docs.docker.com: docker-hub official_images](https://docs.docker.com/docker-hub/official_images/){:target="_blank"}.

[docs.docker.com: commandline cli](https://docs.docker.com/engine/reference/commandline/cli/){:target="_blank"}.

[docs.docker.com: reference builder](https://docs.docker.com/engine/reference/builder/){:target="_blank"}.

[github.com: docker-library](https://github.com/docker-library){:target="_blank"}.

[github.com: official-images](https://github.com/docker-library/official-images){:target="_blank"}.

[github.com: wagoodman dive](https://github.com/wagoodman/dive){:target="_blank"}.

[en.wikipedia.org: docker](https://en.wikipedia.org/wiki/Docker_(software)){:target="_blank"}.

[mooc.fi](https://www.mooc.fi/en/){:target="_blank"}.

[devopswithdocker](https://devopswithdocker.com/){:target="_blank"}.

[why-containers-stop](https://www.tutorialworks.com/why-containers-stop/){:target="_blank"}.

[docker-run-interactive-tty-options](https://www.baeldung.com/linux/docker-run-interactive-tty-options){:target="_blank"}.

[dockerfile-env-variable](https://www.baeldung.com/ops/dockerfile-env-variable){:target="_blank"}.

[what-does-the-input-device-is-not-a-tty-exactly-mean-in-docker-run-output](https://serverfault.com/questions/897847/what-does-the-input-device-is-not-a-tty-exactly-mean-in-docker-run-output){:target="_blank"}.

[quay.io](https://quay.io/){:target="_blank"}.

[docker-workdir-create-a-layer](https://stackoverflow.com/questions/71853912/docker-workdir-create-a-layer){:target="_blank"}.

[understanding-docker-without-losing-your-shit-cf2b30307c63](https://blog.hipolabs.com/understanding-docker-without-losing-your-shit-cf2b30307c63){:target="_blank"}.

[understanding-the-dockerfile-format-3cc6](https://dev.to/aws-builders/understanding-the-dockerfile-format-3cc6){:target="_blank"}.