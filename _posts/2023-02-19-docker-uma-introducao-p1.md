---
layout: post
title:  docker - uma introdução didática - parte 1
date:   2023-03-05 09:15:00
description: introdução à conteinerização com docker e docker-compose, conceitos básicos, comandos e exemplos
tags: ["docker", "containerization"]
language: pt-br
---
## Introdução

Este material traz um compilado de tópicos introdutórios sobre as ferramentas desenvolvidas pela *docker inc*: `docker` e `docker-compose`; com teoria e prática. Elas surgiram e se popularizaram como um conjunto de ferramentas para conteinerização, embora o conceito de contêiner e algumas técnicas já existissem antes. 

Este material se estrutura em três partes e para compô-lo utilizei materiais que estão na seção referências.


* [Primeira Parte](#primeira-parte)
  * [Conceitos bases](#conceitos-bases);
  * [Contexto](#contexto);
  * [Instalação](#instalação);
  * [Primeira execução](#primeira-execução);
  * [Imagem e contêiner](#imagem-e-contêiner);
  * [Básico sobre cliente docker, linha de comando](#básico-sobre-cliente-docker-linha-de-comando);
  * [Executar e encerrar contêineres](#executar-e-encerrar-contêineres);
  * [Mergulhando nas imagens](#mergulhando-nas-imagens);
  * [Construindo imagens](#construindo-imagens);
  * [Definindo condições iniciais do contêiner](#definindo-condições-iniciais-do-contêiner);
  * [Usando volume e portas para interagir com contêineres](#usando-volume-e-portas-para-interagir-com-contêineres);
  * [Permitindo conexões externas com contêineres](#permitindo-conexões-externas-com-contêineres);
  * [Exercícios](#exercícios);
  * [Referências](#referências);
* ~~Segunda Parte~~
* ~~Terceira Parte~~

## Primeira Parte

### Conceitos bases

Contêiner é uma tecnologia de virtualização do tipo de isolamento em nível de sistema operacional (*os-level virtualization*), diferenciando-se das virtualizações do tipo *hipervisionadas* (*hardware hypervisors*), cuja interação com o hardware é intermediada por meio de um software de supervisão. O isolamento de um contêiner ocorre no nível do sistema operacional por meio de técnicas de segregação de instâncias de espaços de usuários. 

*Chroot* é uma das primeiras técnicas, se não a primeira, de isolamento em nível de sistema operacional pela qual processos e seus subprocessos são isolados do resto do sistema de arquivos e processos. Essa técnica consiste em recriar toda a árvore de diretórios necessárias para executar um programa, copiando todos os programas do sistema necessários para executá-lo e ajustando as referências para esse novo simulacro. Em seguida, é utilizado o `chroot` para modificar o diretório `root` para o diretório base recriado, criando um isolamento para o programa executado que não conseguirá acessar nada além do diretório base.

Embora a técnica de *chroot* traga a ideia de isolamento em nível de sistema operacional, a técnica é muito limitada porque não faz isolamento no nível do espaço do kernel, mantendo todos os recursos de hardware compartilhados. As tecnologias mais modernas de contêineres utilizam técnica de isolamento em nível de sistema operacional que é implementada no nível do espaço do kernel, permitindo um isolamento total dos recursos de hardware. Dentre as vantagens desse tipo de técnica estão: redução das despesas (overhead) de recursos necessários para executar um recurso, principalmente se comparado à virtualização hipersivionada, pois os programas que rodam dentro de uma partição virtual usam as interfaces de chamadas de sistema padrões do sistema operacional.

Com isso, *docker* é um conjunto de ferramentas cujo objetivo é implementar virtualização de nível do sistema operacional, com isolamento e mecanismos de gerenciamento de recursos, utilizando `cgroups` e `namespaces`. A base desse conjunto de ferramentas é uma estrutura cliente-servidor: servidor (`docker daemon`), comunicação por `rest api` (`docker engine` `api`), cliente (`docker cli`, `docker desktop`), repositório de imagens (`docker hub`). O serviço de `daemon` do *docker*, também chamado de `dockerd`, é um processo persistente que gerencia os contêineres e manipula objetos vinculados a eles. O `daemon` responde a requisições que chegam pela `api` e que se originam geralmente pela ferramenta de linha de comando.

[⇡](#introdução)

### Contexto

`Docker` é uma consolidada ferramenta quando se pensa em integração e entrega contínuas em ambientes de desenvolvimento de software, pois:

* possibilita isolamento e customização de ambientes, ou seja, no contêiner é instalado apenas o que é necessário para a execução do software principal ou de algum servidor que dará suporte ao software principal;
* o isolamento de contêineres possibilita com que em um mesmo *host* vários contêineres executem diferentes versões de uma linguagem de programação, por exemplo, sem nenhuma interferência entre os próprios contêineres e entres eles e o *host*;
* possibilita criação de ambientes predefinidos que servem de base para a criação de outros contêineres mantendo integridade entre eles. Além disso, suporta infraestrutura como código (*IaC*), `dockerfile`, no qual se especificam várias características para uma imagem que servirá de protótipo para criação de contêineres;
* em relação a uma máquina virtual as despesa (overhead) de recursos necessários para o funcionamento de um contêiner são reduzidas. Nesse sentido, os contêineres são efêmeros porque deve ser possível criálos e destruí-los constantemente e sempre que necessário. Aumentar ou diminuir o número de instâncias de um contêiner é conhecido como escalonamento, tendo como objetivo aumentar ou diminuir recursos (*horizontal scaling*), que por sua vez visa aumentar ou diminuir a disponibilidade de um serviço;
* o caráter efêmero de um contêiner deve ser compreendido de modo pragmático, ou seja, *posso destruir todos eles hoje, mas também se for preciso recriá-los amanhã*;

[⇡](#introdução)

### Instalação

Dois são os pré-requisitos de instalação: `docker` e `docker-compose`, ambos da *docker inc.*. *Docker* possui um rico ambiente de documentação que pode ser acessado [aqui](docs.docker.com){:target="_blank"}. Em se tratando de instalação, mais informações podem ser obtidas [aqui](docs.docker.com/engine/install/){:target="_blank"}. O mesmo vale para [docker-compose](docs.docker.com/compose/){:target="_blank"} e os documentos de [instalação](https://docs.docker.com/compose/install/){:target="_blank"} para ele.

Em se tratando da instalação do `docker` é importante saber que a arquitetura inicial exigia que ela fosse feita como super usuário (root / administrador). A partir da versão 19.03 essa exigência foi alterada, permitindo a instalação com usuário sem privilégios de root / administrador. Tal assunto pode ser visto [aqui](https://docs.docker.com/engine/security/rootless/){:target="_blank"}.

Além disso, é comum que após a instalação do `docker`, no caso em que se opte por executá-lo como super usuário, o usuário responsável por executá-lo seja incluído no grupo `docker`. Assim, não será necessário prefaciar o comando `docker` com `sudo`. Mais detalhes [aqui](https://docs.docker.com/engine/install/linux-postinstall/){:target="_blank"}.

[⇡](#introdução)

### Primeira execução

A melhor forma para validar a instalação do `docker` é realizar uma primeira execução de uma imagem chamada `hello-world`, usando o comando `docker container run hello-world`.

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
~~~

O log nos mostra que o contêiner foi executado com sucesso. Também mostra que foi a primeira vez que a imagem `hello-world` foi usada. Isso fica evidenciado nas cinco primeiras linhas, onde vemos que o `docker` procurou pela imagem no repositório local, sem a encontrar. De fato, vejamos o procedimento:

1. em primeiro lugar, busca-se a imagem no repositório local. No caso acima, não foi encontrada, `Unable to find image 'hello-world:latest' locally`;
2. em segundo, busca-se no repositório público quando não encontrada localmente;
3. por fim, se a imagem não for encontrada em nenhum desses dois locais, o comando não será executado.

O repositório oficial do `docker` encontra-se [aqui](hub.docker.com){:target="_blank"} e nele imagens oficiais de ferramentas, aplicações e sistemas operacionais são armazenados e disponibilizados para uso geral. Ademais, qualquer usuário pode incluir imagens no repositório, seguindo as políticas específicas de assinatura, e nesse sentido imagens não oficiais também estão disponíveis.

Imagens não oficinais são imagens que não tem uma garantia da sua finalidade, pois podem trazer algum malware. Lembre-se que qualquer um pode disponibilizar imagens no repositório do `docker`, sem curadoria da *docker inc*, e o download é feito pela internet. Esses fatores colocam a exigência de garantir que o que está sendo executado é seguro.

As imagens trazem alguns metadados, dentre os quais cabe destacar: `tag` e `digest`. A `tag` é utilizada para indicar a versão da imagem, cuja atribuição é feita no momento da geração. O metadado `tag` pode funcionar como versionamento, quando, por exemplo, uma `tag` nova é gerada quando uma nova versão de uma imagem é compilada. Assim, ao executar `docker container run` poderia escolher entre diferentes versões: `docker container run hello-world:1.0`.

Caso não seja usado `:alguma-tag`, por padrão o `docker` busca a imagem marcada como `:latest`. Dependendo do gerenciamento da imagem, *tags* marcam ciclos de atualização distintos. Por exemplo, na imagem do `kalilinux` existe uma `tag` chamada `kali-rolling` que indica que a imagem é semanalmente atualizada com novas ferramentas e versões. E existe `kali-last-release` que segue uma atualização a cada quatro meses. 

O segundo metadado é `digest` e é usado como um `hash` da imagem. No exemplo acima, a imagem `hello-world` traz `digest:sha256:aa0cc8055b82dc2509bed2e19b275c8f463506616377219d9642221ab53cf9fe`. Essa informação garante que a imagem que chegou localmente é a mesma disponível no `docker hub`, isto é, ela não foi alterada durante o processo de download. Lembremo-nos que essa transferência é feita pela internet e técnicas podem ser utilizadas para modificar o conteúdo que será entregue. Acerca disso, é importante não confundir com os identificadores das imagens. Eles são também *hashes*, porém vinculados ao ambiente local. Nesse sentido, `digest` é uma informação que garante a integridade entre a imagem local e a sua origem.

Por fim, a partir da primeira execução de uma imagem o `docker` não busca mais a imagem no repositório remoto. Ele sempre usurá a imagem local.

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

[⇡](#introdução)

### Imagem e contêiner

Contêineres são instâncias de imagens. Elas são criadas de acordo com uma finalidade que se deseja obter executando um contêiner. Imagens são protótipos para contêineres. 

`Docker` possui uma funcionalidade para compilação de imagens a partir de arquivo de instruções, chamado de `dockerfile`. O conceito de infraestrutura como código, `IaC`, entra em cena. De modo breve, `IaC` é a capacidade que algumas ferramentas tem de criar e configurar ambientes a partir de arquivos de instruções, a partir de código. Com isso, os ambientes são criados e configurados igualmente seguindo as instruções dos arquivos em qualquer ambiente que os hospede. Um arquivo `dockerfile` suporta uma série de instruções, que podem ser conferidas [aqui](https://docs.docker.com/engine/reference/builder/){:target="_blank"}, mas uma versão básica de um arquivo seria:

~~~ dockerfile
FROM <imagem>:<tag>
RUN <instala dependências>
CMD <comandos que são executados quando há uma solicitação de execução de contêiner, `docker container run`>
~~~

A instrução `FROM` é utilizada para referenciar uma imagem mais básica que dará suporte a construção da imagem desejada. Geralmente, uma nova imagem é dependente de outra, cujos recursos são o mínimo necessário. Nesse sentido, o arquivo de instruções modifica a imagem base, *enxuta*, instalando componentes pela instrução `RUN`.

`FROM` e `RUN` são instruções que operam no escopo de uma imagem. Durante a construção de uma imagem, ao executar um desses comandos um contêiner temporário é construído para que seja executado o comando e uma imagem é gerada com as novas modificações e, a partir da qual, as demais instruções do arquivo de instruções serão executadas. Por conta disso, `FROM` e `RUN` são instruções que tem o seu campo de ação delimitado pelo escopo da imagem. Diferente, por exemplo, da instrução `CMD`, pela qual é possível executar comandos no escopo do contêiner gerado.

Com um `dockerfile` configurado, o comando `docker image build` é usado. Esse comando faz uma análise do arquivo de instruções e, caso não ocorra problemas, uma imagem é compilada. Essa é uma das funcionalidades que tornou o `docker` popular. Isso porque com um `dockerfile` é possível automatizar ambientes para a integração e entregas contínuos.

Usando um arquivo `dockerfile`, cada imagem gerada com o mesmo arquivo terá as mesmas características e consequentemente todos os contêineres, independentemente do ambiente que hospeda os recursos. Assim, os contêineres não dependem do ambiente que os hospede, podendo ser levados ao longo de todo o processo de desenvolvimento até a entrega do artefato final. Com isso, se reduz ou quase se elimina a famosa situação *na-minha-máquina-o-problema-não-acontece*.

Contêineres são objetos que devem ser destruídos ou gerados à medida que for necessário. As imagens tem uma duração maior e tendem a ser versionadas quando mudanças não necessárias. Ainda sobre imagens, elas devem ser consideradas imutáveis e com algum grau de efemeridade. Recomenda-se que a cada modificação o arquivo de instruções seja modificado e versionado, e a imagem recriada com nova versão, evitando-se, assim, inconsistências.

Para listar as imagens no ambiente local, usa-se o seguinte comando `docker image ls` ou `docker images`. E como resultado, temos:

~~~ bash
~$ docker images
REPOSITORY                          TAG        IMAGE ID       CREATED          SIZE
hello-world                         latest     feb5d9fea6a5   16 months ago    13.3kB
~~~

Lembrando que `REPOSITORY`, `TAG`, `IMAGE ID`, `CREATED` e `SIZE` são metadados utilizados para descrever um arquivo de imagens.

Ao executar `docker container run hello-world`, o `docker` faz download da imagem para o repositório local. Nas sucessivas execuções, a imagem  local será requisitada sempre que for usado `hello-world` ou `hello-world:latest`. Nos casos em que um volume grande de diferentes imagens são utilizadas constantemente, é importante revisar o repositório local para controlar os recursos dispendidos em disco para isso.

Os contêineres são imutáveis e efêmeros, a criação e destruição deles não deve ser um problema. Por conta disso, alterações que devem persistir em todas as instâncias não devem ser feitas em um contêiner específico, mas no arquivo `dockerfile`, desde o qual gerou-se uma imagem que serve de base para os contêineres. Contêineres são ambientes isolados entre si e do ambiente que os hospeda, exceto quando definidos métodos de interação via stdin/stdout/stderr, volumes compartilhados, e TCP/UDP.

Para listar os contêineres em execução, utiliza-se `docker container ls` ou `docker ps`. E como resultado, pode-se receber uma lista vazio, quando não houver nenhum contêiner em execução.

~~~ bash
$ docker container ls
CONTAINER ID   IMAGE              COMMAND                  CREATED             STATUS             PORTS     NAMES
~~~

No entanto, seguindo o exemplo `docker container run hello-world` nada aparece como resultado ao comando `docker container ls`. Isso porque ao executar a imagem ela atinge o seu objetivo, qual seja, escrever na saída padrão a mensagem *hello-world* e a sua execução é encerrada. Por isso, essa execução não aparece na lista de contêineres em execução. Nesses casos, é preciso executar `docker container ls -a` ou `docker ps -a`.

~~~ bash
$ docker ps -a
CONTAINER ID   IMAGE                 COMMAND           CREATED         STATUS                     PORTS     NAMES
6630496d6fe8   hello-world           "/hello"          4 seconds ago   Exited (0) 3 seconds ago             laughing_carver
~~~

`CONTAINER ID`, `IMAGE`, `COMMAND`, `CREATED`, `STATUS`, `PORTS`, `NAMES` são metadados utilizados para descrever um contâiner.

* `CONTAINER ID` é um hash que identifica um contêiner, sendo muito utilizado em comandos para referenciar um contêiner. Por exemplo, `docker container stop 6630496d6fe8`. É ainda possível utilizar os três primeiros digitos `docker container stop 663`. Caso `663` se repita entre outros contêineres, um outro digito deve ser informado para especificar;
* `IMAGE` indica qual imagem o contêiner usa como base; 
* `COMMAND` indica o comando que é executado quando o contêiner inicia a execução. Geralmente um contêiner tem uma função específica, realizada pela execução de um programa que estará descrito nesse metadado. 
* `STATUS` descreve o estado do contêiner e pode assumir `created`, `restarting`, `running`, `removing`, `paused`, `exited` and `dead`. Nesse exemplo, o estado `exited` indica que o programa executado dentro do contêiner encerrou a execução e retornou um código. Esse código pode indicar execução correta ou incorreta do programa.
* `PORTS` é um metadado que indicará se o contêiner faz alguma ligação por porta com o `host` que o hospeda. Geralmente, programas do tipo servidores farão uma ligação com o `host`, compartilhando o serviço por meio de uma ligação entre portas (porta-do-host:porta-do-contêiner). Nesses casos será indicado qual ip e porta do host e do contêiner estão ligados. 
* `NAMES` é uma alternativa para `CONTAINER ID`. Quando não especificado, `docker` gera um nome por conta própria. Caso se queira atribuir um nome usa-se a *flag* `--name`. 

~~~ shell
$ `docker run --name oi-mundo hello-world`
$ docker ps -a
CONTAINER ID   IMAGE                                             COMMAND                  CREATED         STATUS                     PORTS     NAMES
48222d0fc147   hello-world                                       "/hello"                 7 seconds ago   Exited (0) 6 seconds ago             oi-mundo
~~~

[⇡](#introdução)

### Básico sobre cliente docker, linha de comando

`Docker` funciona em uma arquitetura cliente-servidor: um cliente, geralmente linha de comando, solicita via *rest* *api* para o servidor, *daemon*, que execute algum comando. Embora existam outros clientes, a interface de linha de comando (cli - command line interface) é o cliente mais comum e versátil. As *flags* que adaptam as execuções do comando `docker` podem ser encontradas [aqui](https://docs.docker.com/engine/reference/commandline/cli/){:target="_blank"}.

`docker container run <image>` é o comando mais utilizado para instanciar uma imagem como contêiner. No entanto, internamente o comando `run` executa outros. Primeiro, é verificado se a imagem indicada está no repositório local ou se é preciso obter a imagem do repositório remoto. O comando equivalente é `docker pull <imagem>`. Segundo, o contêiner é criado com base na imagem e, por fim, executado. Respectivamente, são usados os comandos: `docker container create <imagem>` e `docker container start -i <imagem>`.

Contêineres possuem ciclos de vida que são dependentes do objetivo para o qual eles foram criados. Algumas vezes contêineres são criados e executam uma função que os deixam em execução constante. Isso ocorre, geralmente, com contêineres que executam servidores de aplicações, por exemplo, web servers. Em outros casos, como no exemplo utilizado até aqui da imagem `hello-world`, o contêiner executa a função designada no atributo `COMMAND` e encerra o seu ciclo de vida.

Ao encerrar o seu ciclo de vida, um contêiner não é excluído, mas seu status é modificado. Para listá-los usa-se o comando `docker ps -a` e para excluí-los o comando `docker container rm <contêiner-id>` ou `docker rm <contêiner-id>`.

Contêineres e imagens tedem a se acumular no sistema local. Para removê-los usa-se os comandos: `docker image rm hello-world` ou `docker rmi hello-world`, `docker rm <contêiner-id>` ou `docker container rm <contêiner-id>`.

Caso se queira remover imagens em um sistema local, só será possível removê-las se não existir contêineres em qualquer estado. Do contrário, `docker` informará: `Error response from daemon: conflict: unable to remove repository reference "hello-world" (must force) - container <container ID> is using its referenced image <image ID>`. Nesse caso, precisamos remover os contêineres dependentes, mas antes disso é preciso identificá-los. Em cenários com um número muito grande de contêineres, utilizar do filtro com imagem pode facilitar: `docker container ls -a | grep hello-world`.

O objetivo desse filtro é trazer todos os identificadores de contêineres que são dependentes da imagem que queremos remover. Os identificadores, ou simplesmente *ids*, são utilizados pelo `docker` em vários contextos, pois identificam um contêiner isoladamente. Funcionam como um hash. Um recurso que facilita o uso deles é utilizar os três primeiros digitos para referenciar um contêiner. Caso os três primeiros não encontrem apenas um contêiner, acrescenta-se mais um digito. Supondo que o *id* do contêiner a ser removido seja *6630496d6fe8*, bastaria `docker container rm 663`. Acaso `663` retorne mais de um contêiner, deve-se usar `6630` ou mais digitos.

Existem maneiras mais eficientes para remover uma quantidade enorme de contêineres. A mais *radical* é utilizar a *flag* `prune`, como no exemplo `docker container prune`. `Prune` é uma *flag* versátil que pode ser utilizada em vários contextos para fazer uma *limpeza* de objetos que não estão em um estado de execução: `docker image prune`, `docker system prune`.

Ao remover os contêineres dependentes da imagem `hello-world`, será possível remover a imagem usando o comando `docker rmi hello-world`. E para confirmar usamos o comando `docker image ls`. Outro comando importante em relação a imagens é `docker image pull <nome-da-imagem>`, pelo qual imagens podem ser obtidas. E ainda `docker search <nome-da-imagem>` que é usado para localizar imagens no repositório.

Troquemos de exemplo e busquemos uma imagem que execute a aplicação *nginx*, famoso servidor web. Por se tratar de uma aplicação amplamente difundida, é bem provável que exista uma imagem oficial no repositório remoto. Para confirmar, usamos o comando `docker search nginx`:

~~~ bash
NAME                         DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
nginx                        Official build of Nginx.                        17972     [OK]       
linuxserver/nginx            An Nginx container, brought to you by LinuxS…   183                  
bitnami/nginx                Bitnami nginx Docker Image                      150                  [OK]
ubuntu/nginx                 Nginx, a high-performance reverse proxy & we…   75                   
~~~

Os primeiros registros da busca indicam sempre as distribuições oficiais, que devem ter prioridade caso você não tenha confiança em algum fornecedor especifico. Com isso, busca-se garantir integridade da imagem e dos contêineres que serão gerados, reduzindo o risco de gerar algum problema no host ou na aplicação dependentes do contêiner. O campo *stars* também pode ser um critério de confiança. Ele indica a relevância da imagem na comunidade.

Escolhida a imagem, já sabemos como proceder: `docker container run ngix`. Essa é a maneira mais simples, embora nesse comando existam outros processos subjacentes, tais como: `docker pull ngix`, `docker create ngix`, `docker container start ngix`.

Ao executar `run nginx` o terminal congela logo após iniciar o contêiner. Isso é o comportamente padrão do `docker` em contêineres que executam tarefas persistentes. Nesses casos, o terminal fica vinculado ao processo que foi iniciado. Se executar o comando `docker ps` em outro terminal, o contêiner `nginx` estará em execução. 

Em alguns cenários, opta-se por executar o contêiner desvinculando-o do terminal. Para isso, usa-se o comando `docker container run -d nginx`. A flag `-d` é a forma simplificada de `--deatach`. Ele modifica o comportamento do comando `docker run` para que o contêiner seja executado em *background*. Quando essa flag é usada, apenas é impresso o identificador do contêiner no terminal.

Contêineres que executam processos persistentes podem ser colocados em *background*, desvinculando-o do terminal que o executou: `docker container run -d nginx`. Parar um contêiner: `docker container stop <id ou nome>`. Remover um contêiner: `docker container rm <id ou nome>`, e uma imagem: `docker imagem rm nginx`.

Em cenários com constante criação e destruição de contêineres, com o tempo é comum que o `docker` fique *entupido* de contêineres e imagens. Para resolver isso é importante fazer uma *limpeza* usando a *flag* `prune`: `docker container prune`, `docker image prune`. Ou ainda com a *flag* `--rm` junto ao comando `docker run`. Desse modo, `docker` removerá o contêiner logo em seguida ao fim de sua execução. Exemplo: `docker container run -it --rm ubuntu`.

[⇡](#introdução)

### Executar e encerrar contêineres

Há diferentes *flags* que modificam o modo como um contêiner é executado e como ele interage com o terminal que o executou. A maneira mais simples para executar um contêiner é `docker run ubuntu`. Há, no entanto, implícito nesse comando outros que compõe o ciclo de criação de um contêiner. 

Embora a imagem seja de um sistema operacional, o contêiner será encerrado logo após a inicialização. Talvez estivêssemos esperando um comportamente similar a uma máquina virtual, que permanece executando o sistema operacional mesmo que nenhum processo específico seja executado. Contêineres possuem despesas contínuas menores do que em um ambiente virtualizado. Despesas contínuas são despesas que não estão ligadas diretamente a um serviço, processo ou aplicação. Contêineres isolam funções específicas e a executam, incluindo todas as dependências para isso. Em cenários em que a função não é contínua, ao encerrar a função, o contêiner se encerra.

No caso da imagem `ubuntu`, ela foi construída para executar um processo de bash quando inicializada sem um comando específico. Confirmamos essa hipótese no registro de execução do contêiner.

~~~ shell
docker ps -a

CONTAINER ID   IMAGE              COMMAND                  CREATED         STATUS                     PORTS     NAMES
d1a6ef4dc22b   ubuntu             "bash"                   4 seconds ago   Exited (0) 3 seconds ago             eager_pike
~~~

O comando `bash` foi executado no contêiner, encerrando-se com código `0`, indicando sucesso. Códigos de execução dos processos não dependem do contêiner, mas do retorno do programa executado. O contêiner se encerrou e não permitiu nenhuma interação, como se nada tivesse acontecido.

~~~ shell
$ docker run ubuntu echo "echoing"
echoing

$ docker ps -a
CONTAINER ID   IMAGE                                                          COMMAND                  CREATED          STATUS                      PORTS     NAMES
0bd64f012925   ubuntu                                                         "echo echoing"           14 seconds ago   Exited (0) 13 seconds ago
~~~

Nessa outra tentativa, obtivemos uma interação. O contêiner se encerra imprimindo no console a palavra *echoing*, ou seja, o comando `echo echoing` foi executado. Por padrão, `docker run` não conecta o *stream* padrão de entrada (*sdtin*) do contêiner com o terminal, mas conecta o *stream* padrão de saída (*stdout*) e o *stream* padrão de erro (*sdterr*). Por isso recebemos o resultado da execução do comando. Nesse cenário, em casos em que o processo exige interação de entrada, o processo é encerrado imediatamente. Como foi o caso quando executado `bash`. Ou ainda no próximo caso.

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

Mas nem sempre a entrada padrão virá de um terminal. Pode-se, por exemplo, vincular um *pipe* de um retorno de um comando, tal como `echo "essa entrada vem de um pipe"`, como no exemplo a seguir.

~~~ shell
$ echo "essa entrada vem de um pipe" | docker run -i ubuntu cat
essa entrada vem de um pipe
~~~

Ou ainda este outro caso, em que a entrada padrão do contêiner é vinculada ao terminal, recebendo um comando atribuído como parâmetro e executando-o. A primeira linha é entrada de dados, a segunda, o retorno do comando `cat`. Como foi atribuída a *flag* `-i`, o comando `cat` *prende* a execução, aguardando a interação.

~~~ shell
$ docker run -i ubuntu cat
docker test
docker test
test docker
test docker
~~~

Nesse último caso, a entrada padrão do contêiner foi vinculada ao terminal que executou o processo. A primeira linha *docker test* foi digitada, a segunda, foi resultado do comando `cat`. Um último exemplo:

~~~ shell
$ docker run -i ubuntu rev
1234567890
0987654321
~~~

Embora nesses exemplos tenhamos conseguido interagir com comandos executados dentro do contêiner, enviando e recebendo dados, em certas situações é necessário interagir com um terminal dentro do contêiner. Nesse caso, é necessário alocar um *pseudo-TTY*, ou seja, um terminal emulado. Para isso, a *flag* utilizada é `--tty` ou `-t`.

~~~ shell
$ docker run -t ubuntu
root@c197f5486a62:/# 
~~~

No exemplo acima, foi requisitado um terminal para o `docker` e nos foi entregue `root@c197f5486a62`. No entanto, nenhum *stream* de entrada foi vinculado ao *stream* de entrada padrão do contêiner. Isso fica mais explícito quando usamos *pipe* para vincular um *stream*.

~~~ shell
$ echo "from echo" | docker run -t ubuntu cat
~~~

O retorno é uma linha em branco. Embora o cat tenha se conectado ao terminal emulado, o terminal emulado não se conectou à entrada do *pipe* porque não foi designado ao `docker` que ele exposse o *stream* de entrada padrão para receber entrada de dados externo. Lembremos que um contêiner é totalmente isolado, exceto se configurado para se conectar com o *mundo exterior*, e exceto pelo *sdtout* e *sdterr*.

Com isso, no geral e na maioria das vezes é interessante criar o contêiner com `-it`, `docker create -it ubuntu` ou `docker run -it ubuntu`. Caso se opte por não usar `docker run`, quando o contêiner for executado, deve-se vincular o terminal ao contêiner `docker start -ai <id>`. 

Apenas como registro, `-it` é a forma simplificada para `--interactive` e `--tty`. 

`docker run -d -it --name looper ubuntu sh -c 'while true; do date; sleep 1; done'` é um comando que traz uma complexidade maior, embora conheçamos algumas das *flags*; `-d`, `-it`; e parâmetros `sh -c 'while true; do date; sleep 1; done'`. 

* `-d` executa o contêiner em *background*;
* `-it` cria um terminal e abre o *stream* padrão de entrada do contêiner que poderá ser vinculado ao terminal;
* `--name` cria o contêiner com um nome inicial, embora quando não informado o comando `docker` gera um nome aleatório. O nome do contêiner é um substituto para o *id*, podendo explicitar mais claramente a função do contêiner;
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

O contêiner continua em execução, `ctrl + c` apenas desconecta do *sdtout*. Em um contêiner em execução, podemos criar um novo processo de terminal e acessá-lo.

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

Com `ps aux` notamos que nosso processo inicial está em execução. Algumas vezes usar `docker stop` pode ser lento, pois ele segue um tempo de espera para garantir que todos as aplicações se encerrem depois de um `SIGTERM` com `SIGKILL`. Caso seja preciso encerrar um contêiner mais rapidamente, há algumas opções.

~~~ shell
docker kill looper
docker rm looper

docker run -d --rm -it --name looper-it ubuntu sh -c 'while true; do date; sleep 1; done'
~~~

As duas primeiras linhas encerram a execução de um contêiner e removem-no. A terceira é uma maneira de criar um contêiner de tal modo que ao encerrar o processo principal, o contêiner é removido.

[⇡](#introdução)

### Mergulhando nas imagens

Imagens são protótipos básicos para construção de outras imagens e instanciação de contêineres. Há dois lugares básicos de armazenamento de imagens: repositório local e repositório público do *docker*. E como já vimos é possível localizar imagens no repositório público pelo comando `docker search postgres`.

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

Além do *ok* na coluna *official*, imagens oficiais não possuem o prefixo da organização. Já `bitnami/postgresql` não é uma imagem oficial, o nome está com o prefixo da organização, mas a coluna `automated` está como *ok*, o que indica que essa imagem é construída diretamente do repositório fonte.

`Docker hub` é o repositório oficial do `docker` para imagens oficiais e não-oficiais. No entanto, ele não é o único. Atualmente existe `quay.io`, vinculado à `red hat`, que disponibiliza imagens para `docker`, `podman` e `rkt`. Embora não se possa utilizar `docker search` é possível indicar um endereço para o contêiner `docker pull quay.io/nordstrom/hello-world:2.0`.

Todas as imagens possuem uma *tag* como, por exemplo, *latest*, que indica que é a última versão contruída e enviada ao repositório. Mas cada mantenedor estabele seus próprios critérios em relação as *tags* e o que elas indicam. Por exemplo, a imagem *ubuntu*, além de *latest*, possui uma série de outras tags, dentre elas, algumas que indicam versões específicas, tais como `ubuntu:23.04`, `ubuntu:22.10`, `ubuntu:22.04`, `ubuntu:18.04`.

~~~ shell
$ docker pull ubuntu:23.04
23.04: Pulling from library/ubuntu
2627e5235478: Pull complete 
Digest: sha256:2ca8fe42bcc2979f66dd80c2987a43cfc5502626094b7f838f89759173f3956b
Status: Downloaded newer image for ubuntu:23.04
docker.io/library/ubuntu:23.04
~~~

Imagens são construídas por diferentes camadas e metadados. Em algumas circunstâncias as camadas podem otimizar o processo de obtenção de uma outra imagem, quando essa compartilha uma camada base que já está disponível localmente. Geralmente essa camada base é outra imagem. Imagens permitem *tags* e podem ser entiquetadas com o comando `docker tag`. O nome de uma imagem é composto de três partes: `repositório/organização/imagem:tag`. As imagens que são oficinais, são indicadas apenas por `imagem:tag`.

[⇡](#introdução)

### Construindo imagens

Uma das grandes funcionalidades do `docker` é permitir a criação de imagens customizadas com o arquivo de instruções `dockerfile`. Sigamos um experimento sobre isso.

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

A imagem acima irá executar o programa `docker-custom.sh`, mas para isso ela precisa de uma base que execute arquivos `.sh`. Geralmente, uma imagem se baseia em outra que tem uma versão *enxuta* do linux, a partir da qual outras ferramentas são instaladas para atingir o propósito ao qual os contêineres gerados a partir dela se destinam. `Dockerfile` é um arquivo texto, então podemos usar editor de texto para escrever nele.

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

Vamos compilar uma imagem usando `docker build`, onde `-t hello-docker-custom` será o nome da imagem, o qual poderia ser acrescido de uma outra *tag* tal como `-t hello-docker-custom:v1` ou `-t hello-docker-custom:latest`. `.` designa o local do arquivo `dokerfile`.

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

Na construção da imagem acima podemos notar as camadas que a compõem. As camadas tem multiplas funções. Por um lado, pode ser interessante limitar o número de camadas para reduzir o espaço gasto com armazenamento, porém cada camada funciona como *cache* durante a construção de imagens. Caso apenas a última linha do *dockerfile* seja editada, o comando que faz a construção da imagem inicia a partir da camada editada e pula as camadas das seções anteriores. Isso permite construir imagens que podem ser usadas em *pipelines* mais rápidos.

Há duas formas de modificarmos a imagem `hello-docker-custom`. Uma das opções é modificar o arquivo de instruções, incluindo nele as modificações desejadas. A segunda opção é modificar um contêiner gerado pela imagem e criar uma imagem a partir do contêiner modificado. A primeira opção é considerada a prática mais sustentável e adequada para automação de contêineres, pois o arquivo de intruções é de fácil análise e checagem de integridade. Não obstante, sigamos com um exemplo do segundo cenário.

O objetivo desse exemplo é incluir um arquivo no contêiner executado a partir da imagem `hello-docker-custom` e, em seguida, gerar uma imagem a partir do contêiner modificado.

1. acessar o shell de uma instância de contêiner porque a nossa imagem não tem um processo constante;
~~~ shell
$ docker run -it hello-docker-custom sh
~~~

2. em outro terminal, criar o arquivo que queremos copiar para o contêiner;
~~~ shell
$ touch additional-file
$ echo "test file new image" > additional-file
~~~

3. copiar o arquivo usando o comando de cópia do `docker`, pelo qual é possível copiar arquivos externos diretamente para dentro do contêiner;
~~~ shell
$ docker ps
CONTAINER ID   IMAGE                        COMMAND                  CREATED         STATUS             PORTS     NAMES
7d00fcbd2b12   hello-docker-custom          "sh"                     7 minutes ago   Up 7 minutes                 sharp_northcutt


$ docker cp ./additional-file sharp_northcutt:/usr/src/app/
~~~

4. confirmar a cópia no terminal do passo 1;
~~~ shell
/usr/src/app # ls -l
total 8
-rw-rw-r--    1 1001     1001            20 Jan 25 21:44 additional-file
-rwxrwxr-x    1 root     root            32 Jan 25 14:10 docker-custom.sh
/usr/src/app # 
~~~

5. listar as modificações em um contêiner comparando com a imagem base usando o comando `docker diff`. Nesse exemplo estamos usando as três primeiras letras do *id*. O comando retorna uma lista, identificando as mudanças com as letras A = adicionado, D = deletado, C = modificado. `A /root/.ash_history` é o arquivo de histórico criado pelo shell, e como consequência o diretório `C /root` aparece como modificado. O mesmo vale para o arquivo adicionado `A /usr/src/app/additional-file`, desde o qual os subdiretórios também aparecem como modificados.
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

No entanto, como mencionado, essa técnica é a menos adequada para manter a confiabilidade e integridade das imagens e dos contêineres em um ambiente de integração e entregas contínuos. O melhor é introduzir a alteração no arquivo de intruções e compilar uma nova versão da imagem.

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

[⇡](#introdução)

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
2. estamos com um contêiner executando ubuntu. No entanto, as imagens de sistemas operacionais são geralmente *enxutas*, com poucas aplicações, inclusive as mais básicas, garantindo com que no contêiner seja instalado apenas o que é necessário para o seu propósito. Uma das ferramentas que precisaremos é o `curl`, utilizado basicamente para transferir dados através de vários protocolos, e dentre esses o `http`. Precisaremos dele para que na hora de montar o contêiner a aplicação *youtube-dl* seja obtida do seu endereço web oficial.
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
5. agora nossa aplicação pode executar. Mas, na sua primeira execução, somos alertados sobre a variável de ambiente LC_ALL, cuja função é não restringir o nome dos arquivos por caracteres inválidos.
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

Todo arquivo `dockerfile` deve iniciar com a instrução `FROM`, exceto pelo uso da instrução `ARG`. `FROM` marca um novo estágio da criação da imagem que serve de base para todas as instruções subsequentes. A referência a imagem base, por exemplo, `ubuntu`, pode receber uma *tag*, *18.04*, ou um *digest*. `AS <name>` pode ser usado para nomear o novo estágio da criação da imagem.

A partir do momento em que a instrução `WORKDIR` é atribuída, as instruções `RUN`, `CMD`, `ENTRYPOINT`, `COPY`, `ADD` usam como referência o diretório atribuído a ela. Caso não seja especificado `WORKDIR` será `/`.

`RUN` executará qualquer comando em uma nova camada sobre a imagem atual e registrará os resultados como uma nova imagem base que servirá de base para as próximas instruções do `dockerfile`. Nesse sentido, `RUN` é uma instrução que executa em tempo de contrução das imagens, o que é ideal, por exemplo, para incluir pacotes em uma imagem. Há duas formas de se usar a instrução: `RUN <command>`, conhecida como formato `shell`, ou `RUN` `["executable", "param1", "param2"]`, conhecida como formato `exec`. Esta última exige que se use aspas duplas ao invés de simples, e que se use caracteres de escape como, por exemplo, `RUN ["c:\\windows\\system32\\tasklist.exe"]`. Essas exigências surgem porque no formato *exec* os valores são colocados em um json e tratados. No caso for formato *shell* é possível utilizar `\` para quebra de linhas, tal como no exemplo abaixo.

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

Por fim, `CMD` e `ENTRYPOINT`. Ambas as instruções permitem definir comandos que serão executados quando o contêiner for executado. `ENTRYPOINT` define um executável principal que poderá ser combinado com argumentos recebidos por parâmetro no momento da execução do contêiner, tal como `$ docker run youtube-dl https://imgur.com/JY5tHqr`.

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

[⇡](#introdução)

### Usando volume e portas para interagir com contêineres

Embora um contêiner seja uma unidade isolada, existe uma série de maneiras de interagir com ele. Nesta seção analizaremos duas maneiras: volume e portas.

A imagem que criamos na seção anterior tem como finalidade isolar uma aplicação de download de videos. Seria interessante, então, que o usuário no host conseguisse acessar os vídeos de modo simples. Uma forma de viabilizar isso é ligar um volume externo com um volume interno.

A técnica é chamada de *bind mount* e é feita pelo comando *docker run*: `docker run -v "$(pwd):/mydir" youtube-dl https://imgur.com/JY5tHqr`. Ademais, esssa ténica pode ser usada para conectar contêineres que precisam compartilhar arquivos entre si.

Outra estratégia é compartilhar um arquivo específico, por exemplo, um arquivo de configurações: `-v $(pwd)/config.file:/mydir/config.file`. Com isso, modificações no arquivo a partir do host refletem no contêiner.

[⇡](#introdução)

### Permitindo conexões externas com contêineres

A segunda técnica é chamada de *port bind* e é muito utilizada para disponibilizar serviços que estão executando no contêiner para acesso externo, enviando ou recebendo mensagens para endereços *url* tal como `http://127.0.0.1:4000`. Isso é possível através de um mapeamento entre uma porta do computador host e uma porta do contêiner. A abertura de uma porta do lado de fora para o contêiner acontece em dois passos:

1. exposição de porta (exposing port);
2. publicação de porta (publishing port).

Exposição de porta indica que o contêiner passa a *escutar* em uma determinada porta e para isso utiliza a instrução `EXPOSE <porta>` no `dockerfile`. Publicar uma porta indica ao *docker* para mapear portas do host para as portas do contêiner e é feito ao executar o contêiner: `-p <host-porta>:<container-porta>`.

Também é possível limitar as conexões para certo protocolo: `EXPOSE <porta>/udp` e `-p <host-porta>:<container-porta>/udp`.

Expor portas é uma questão crítica na segurança de ambientes e o modo como isso é feito pode ocasionar que qualquer um tenha acesso à porta exposta. Uma forma de reduzir a área de exposição é forçar que a ligação seja feita apenas com um host específico, tal como: `-p 127.0.0.1:33456:4000`. Assim, apenas conexões do localhost podem ser feitas. Menos recomendado é `-p 3456:3000`, que é igual a `-p 0.0.0.0:33456:4000`, significando que a porta `4000` está aberta para qualquer um.

[⇡](#introdução)

## Exercícios

### Execício 1

Testar a instalação e a permissão do `docker` usando imagem teste `hello-world`. Primeiro usando o comando `docker run`. 

<details>
<summary>Comandos</summary>
{% highlight shell %}
$ docker run hello-world
{% endhighlight %}
</details><br>

Caso não ocorra problemas na execução, esse contêiner imprime na tela a frase *Hello from Docker!* com uma explicação de como essa mensagem foi gerada usando o `docker`. No entanto, o comando `docker run` é uma simplificação de alguns outros comandos. Antes de executar cada um dos comandos na ordem correta para executar o contêiner, vamos remover os objetos criados por `docker run hello-world`.

Respeitando a ordem de dependências, remova contêineres dependetes da imagem `hello-world`, em seguida a imagem.

<details>
<summary>Comandos</summary>
{% highlight shell %}
$ docker ps -a # listar o contêiner para obter o id
$ docker rm <contêiner-id>
$ docker rmi hello-world
{% endhighlight %}
</details><br>

Executar na ordem correta os comandos utilizados pelo comando `docker run`, para executar um contêiner baseado na imagem `hello-world`.

<details>
<summary>Comandos</summary>
{% highlight shell %}
$ docker images hello-world # verificar se a imagem existe localmente. 
$ docker search hello-world # como removemos a imagem local, procurar no docker hub.
$ docker pull hello-world 
$ docker create hello-world
$ docker ps -a # listar o contêiner criado
$ docker start -i <contêiner-id>
{% endhighlight %}
</details><br>

Utilizando os comandos separadamente, o rasultado deve ser o mesmo. Por fim, remova os objetivos criados.

<details>
<summary>Comandos</summary>
{% highlight shell %}
$ docker ps -a # listar o contêiner para obter o id
$ docker rm <contêiner-id>
$ docker rmi hello-world
{% endhighlight %}
</details><br>

### Exercício 2

`Alpine` é uma das mais leves distribuições do `Linux` e por isso tornou-se uma das mais populares distribuições usadas em imagens para o `docker`, como suporte para outras imagens. Primeiro, vamos verificar se a imagem `alpine` existe localmente e no repositório remoto.

<details>
<summary>Comandos</summary>
{% highlight shell %}
$ docker images alpine # verficar se a imagem já está disponível localmente.
$ docker search alpine # verficar se a imagem está disponível para download no repositório remoto.
{% endhighlight %}
</details><br>

Agora vamos obter a imagem `alpine`, isto é, fazer o download dela do repositório remoto para o local, deixando-a disponível para criação de contêineres. E verficar se ela está disponível localmente.

<details>
<summary>Comandos</summary>
{% highlight shell %}
$ docker image pull alpine 
$ docker image ls | grep alpine
{% endhighlight %}
</details><br>

Com a imagem disponível localmente, é possível instanciar contêineres baseados nela. Vamos executar um contêiner baseado na imagem `alpine` e solicitar que seja executado no contêiner o comando `ls -l`.

<details>
<summary>Comandos</summary>
{% highlight shell %}
$ docker container run alpine ls -l
{% endhighlight %}
</details><br>

O comando `docker run` cria uma instância de um contêiner baseado na imagem `alpine` e executa dentro do contêiner o comando `ls -l`. O resultado do comando é devolvido para o terminal que solicitou, imprimindo na tela.  Não é necessário utilizar as *flags* `-it` porque por padrão o docker conecta o *sdtout* do contêiner com o terminal que solicitou a execução. Como já é conhecido, depois da execução do comando no contêiner, o contêiner se encerra porque a sua função foi concluída.

Execute um contêiner baseado na imagem `alpine` e solicite que seja executado o comando `echo "olá do contêiner alpine"` no contêiner.

<details>
<summary>Comandos</summary>
{% highlight shell %}
$ docker container run alpine echo "olá do contêiner alpine"
{% endhighlight %}
</details><br>

Agora, rode o comando `/bin/sh` no contêiner.

<details>
<summary>Comandos</summary>
{% highlight shell %}
$ docker container run alpine /bin/sh
{% endhighlight %}
</details><br>

Nas duas primeiras execuções, os comandos `ls -l` e `echo "olá do contêiner alpine"` são executados e o retorno de cada um retornado para o terminal que os executou. No último caso, o comando `/bin/sh` executa sem nenhum retorno porque o comando deveria abrir um terminal. No entanto, ao executar `docker run` não foi vinculado um terminal de entrada e saída. 

Execute o comando `docker run` com shell interativo.

<details>
<summary>Comandos</summary>
{% highlight shell %}
$ docker container run -it alpine /bin/sh
{% endhighlight %}
</details><br>

Como comparação, máquinas virtuais emulam todo uma pilha de hardware, boot e sistema operacional para então executar algum programa ou função específica. `Docker` funciona diretamente em nível de aplicação, tornando-o mais dinâmico no ciclo de criação e destrução de contêineres.

Visualize os contêineres executados até agora.

<details>
<summary>Comandos</summary>
{% highlight shell %}
$ docker container ls -a # versão completa.
$ docker ps -a # versão simplificada.
{% endhighlight %}
</details><br>

### Exercício 3

Uma das características mais importantes da contêinerização com docker é o isolamento dos contêineres. Cada contêiner tem um sistema de arquivos separado e executa em um *namespace* diferente. Por padrão, os contêineres não interagem entre si, mesmo que utilizem a mesma imagem.

Execute um contêiner usando a imagem `alpine`, com shell interativo e solicite a execução do comando `/bin/ash`. Ao iniciar o contêiner, crie um arquivo chamado `isolamento.txt` com o conteúdo *contêiner isolado*.

<details>
<summary>Comandos</summary>
{% highlight shell %}
$ docker container run -it alpine /bin/ash
/ # echo "contêiner isolado" > isolamento.txt
/ # ls -l isolamento.txt
{% endhighlight %}
</details><br>

Em um outro terminal, crie um contêiner baseado na imagem `alpine` e solicite a execução do comando `ls`

<details>
<summary>Comandos</summary>
{% highlight shell %}
$ docker container run alpine ls
{% endhighlight %}
</details><br>

Como esperado, na execução do segundo contêiner o arquivo `isolamento.txt` não é listado. Isso é o isolamento dos contêineres e uma das principais características de segurança do `docker`. No entanto, no dia-a-dia isolamento permite com que usuários rapidamente criem cópias de teste de aplicações separadas, isoladas e rodando lado a lado sem interferência. 

### Exercício 4

Os exercícios anteriores criaram muitos objetos. Remova os contêineres e as imagens usadas acima.

<details>
<summary>Comandos</summary>
{% highlight shell %}
$ docker container prune
$ docker rmi alpine
$ docker rmi hello-world
{% endhighlight %}
</details><br>

### Exercício 5

Use a imagem `devopsdockeruh/simple-web-service:ubuntu` para criar um contêiner. Acesse o contêiner pelo terminal e leia o arquivo `./text.log`.

<details>
<summary>Comandos</summary>
{% highlight shell %}
# O contêiner será executado desvinculado (-d) do terminal, 
# mas com a possibilidade de vinculação (-it). 
# O nome dele será fixo --name log-webserver
$ docker container run -d -it --name log-webserver devopsdockeruh/simple-web-service:ubuntu
#
# verifica se o contêiner foi criado corretamente.
$ docker ps | grep log-webserver
#
# podemos verificar se o contêiner está rodando o programa principal dele `/usr/src/app/server`.
# `--no-stdin` não vincula o stream de entrada de dados ao terminal que executou o comando.
# apenas é vinculado o stdout. Com isso, podemos usar o comando `ctrl+c` para encerrar
# a visualização do stream de saída sem interromper o contêiner. 
# Outra opção, sem usar `--no-stdin`, é a sequência `ctrl+p` e `ctrl+q`
$ docker attach --no-stdin log-webserver
#
# mas o objetivo é acessar outro arquivo `./text.log`.
# É preciso acessar o sistema de arquivos do contêiner.
$ docker exec -it log-webserver bash
#
# acessado o ambiente do contêiner, basta ler o arquivo.
root@df1404fa4f3a:/usr/src/app# tail -f ./text.log
{% endhighlight %}
</details><br>

Esta imagem `devopsdockeruh/simple-web-service:alpine` tem a mesma função que a anterior, embora seja baseada em `alpine`. Faça download e compare os tamanhos entre elas. `Alpine` é uma das mais enxutas imagens com linux. Em seguida, faça a mesma tarefa designada acima.

<details>
<summary>Comandos</summary>
{% highlight shell %}
#
# primeiro fazer o download da nova imagem.
$ docker image pull devopsdockeruh/simple-web-service:alpine
#
# agora vamos listar as imagens e comparar os tamanhos. Como dissemos, a versão `alpine` é consideravelmente mais leve.
#
$ docker images | grep devopsdockeruh
# agora podemos seguir os passos utilizados na primeira imagem.
# O contêiner será executado desvinculado (-d) do terminal, 
# mas com a possibilidade de vinculação (-it). 
# O nome dele será fixo --name log-webserver-alp
$ docker container run -d -it --name log-webserver-alp devopsdockeruh/simple-web-service:alpine
#
# verifica se o contêiner foi criado corretamente.
$ docker ps | grep log-webserver-alp
#
# podemos verificar se o contêiner está rodando o programa principal dele `/usr/src/app/server`.
# `--no-stdin` não vincula o stream de entrada de dados ao terminal que executou o comando.
# apenas é vinculado o stdout. Com isso, podemos usar o comando `ctrl+c` para encerrar
# a visualização do stream de saída sem interromper o contêiner. 
# Outra opção, sem usar `--no-stdin`, é a sequência `ctrl+p` e `ctrl+q`
$ docker attach --no-stdin log-webserver-alp
#
# mas o objetivo é acessar outro arquivo `./text.log`.
# É preciso acessar o sistema de arquivos do contêiner.
$ docker exec -it log-webserver-alp bash
#
# acessado o ambiente do contêiner, basta ler o arquivo.
root@df1404fa4f3a:/usr/src/app# tail -f ./text.log

{% endhighlight %}
</details><br>

### Exercício 6

Instanciar um contêiner baseado em uma imagem `ubuntu`, passando como argumento `sh -c 'echo "Input website:"; read website; echo "Searching.."; sleep 1; curl http://$website;'`. Entre as aspas estão vários comandos de terminal. Dentre eles: `read website` e `curl http://$website;`.

O objetivo é fazer com que a instância do contêiner interaja com o usuário no terminal que executou o comando `docker run`, pedindo uma url que será lida pelo comando `read`, atribuída a uma variável de ambiente `website` e utilizada no comando `curl`.

<details>
<summary>Comandos</summary>
{% highlight shell %}
#
# ao invés de utilizar o comando run diretamente, primeiro vamos verificar se existe uma imagem ubuntu local.
$ docker images ubuntu
#
# considerando que o comando acima retorne vazio, vamos procurar por uma imagem no repositório remoto do docker
$ docker search ubuntu
#
# esse comando deve retornar várias linhas. entre essas, a primeira deve indicar a imagem oficial do ubuntu, chamada `ubuntu`. 
# vamos fazer o download dessa imagem.
#
$ docker image pull ubuntu
#
# feito isso, precisamos modificar o comando  'echo "Input website:"; read website; echo "Searching.."; sleep 1; curl http://$website;'` para que ele solicite a instalação do `curl` e a chamada do comando `docker run` passando uma variável de ambiente. Além disso, `-it` deve ser incluído para que o contêiner interaja com o terminal.
#
$ docker container run -it --env website ubuntu sh -c 'apt update && apt upgrade -y && apt install curl -y;echo "Input website:"; read website; echo $website; curl $website;'
{% endhighlight %}
</details><br>

Importante limpar os objetos criados após atingir o objetivo do exercício.

<details>
<summary>Comandos</summary>
{% highlight shell %}
$ docker container prune
{% endhighlight %}
</details><br>

[⇡](#introdução)

### Exercício 7

Neste exercício usaremos a imagem `devopsdockeruh/pull_exercise`. O programa principal que roda nela solicita que o usuário informe uma senha no terminal. A senha está em um arquivo no sistema de arquivos do contêiner.

<details>
<summary>Comandos</summary>
{% highlight shell %}
#
# como precisamos interagir com o contêiner, a execução precisa das *frags* `-it`.
$ docker run -it --name=pull-exercise devopsdockeruh/pull_exercise
#
# feito isso, o terminal mostrará a mensagem "Give me the password: ".
#
# para descobrir a senha, precisar acessar o contêiner por um outro terminal. Essa imagem não possui o `bash`, precisamos usar `sh`.
$ docker exec -it pull-exercise sh
#
# por onde começamos a procurar? o comando padrão que está executando na imagem pode ser visto utilizado `docker ps`
$ docker ps
#
# sabemos agora que a imagem roda `node index.js`. analisando o arquivo `index.js`
$ cat /usr/app/index.js
#
# no código, a senha correta é "basics".
{% endhighlight %}
</details><br>

### Exercício 8

Baseado na imagem `devopsdockeruh/simple-web-service:alpine` construa outra imagem chamada `web-server` que rode o servidor web (/usr/src/app/server). Ao fim, o servidor deve iniciar quando for executado `docker run web-server`.

<details>
<summary>Comandos</summary>
{% highlight shell %}
#
# há dois modos para se criar uma imagem: modificar um contêiner e a partir do contêiner criar uma imagem, 
# criar um arquivo `Dockerfile` como base. Neste exercício vamos usar um Dockerfile.
# O arquivo irá usar duas instruções `FROM` e `CMD`. Com a instrução `FROM` vinculamos a nova imagem a base, na qual está o web-server.
# Com a instrução `CMD`, indicamos qual programa irá executar quando o contêiner for iniciado.
#
$ echo "FROM devopsdockeruh/simple-web-service" > web-server-dockerfile
$ echo >> web-server-dockerfile 
$ echo "CMD server" >> web-server-dockerfile
#
# confirmar que o arquivo foi gerado.
$ cat web-server-dockerfile
#
# construir a imagem com a *tag* `web-server:latest`, usando o arquivo web-server-dockerfile.
$ docker build -t web-server -f web-server-dockerfile .
#
# executar o servidor web passado como argumento na chamado do docker run.
$ docker run web-server
{% endhighlight %}
</details><br>

### Exercício 9

[No exercício 6](#exercício-6) passamos os comandos `echo "Input website:"; read website; echo "Searching.."; sleep 1; curl http://$website;` para um contêiner através do comando `docker run`. O objetivo, agora, é criar um Dockerfile baseado na imagem `ubuntu:20.04`, com as dependências instaladas para rodar a *string* de comandos acima. Esses comandos devem estar em um script, `.sh`, que deve ser copiado para a imagem e executado usando a instrução `CMD`.

<details>
<summary>Comandos</summary>
{% highlight shell %}
#
# primeira parte, criar arquivo curler.sh com o conteúdo `echo "Input website:"; read website; echo "Searching.."; sleep 1; curl http://$website;`.
$ touch curler.sh
$ echo "#!/bin/bash" > curler.sh
$ echo "echo 'Input website:'" >> curler.sh
$ echo "read website" >> curler.sh
$ echo "echo 'Searching..'" >> curler.sh
$ echo "sleep 1" >> curler.sh
$ echo "curl http://$website" >> curler.sh
#
# criar o arquivo curler-dockerfile.
$ touch curler-dockerfile
$ echo "FROM ubuntu:20.04" > curler-dockerfile
$ echo "RUN apt update && apt upgrade -y && apt install curl -y" >> curler-dockerfile
$ echo "COPY ./curler.sh" >> curler-dockerfile
$ echo "RUN chmod +x curler.sh" >> curler-dockerfile
$ echo "CMD /curler.sh" >> curler-dockerfile
#
# montar a imagem com a tag curler
$ docker build -t curler -f curler-dockerfile .
$ docker run -it curler
{% endhighlight %}
</details><br>

### Exercício 10

[No exercício 9](#exercício-9) usamos a instrução `CMD` para rodar o comando `curl`. Com essa abordagem, o script torna-se responsável por solicitar a url. Se utilizarmos a instrução `ENTRYPOINT` para rodar o comando `curl`, podemos passar a url quando rodamos o comando `docker run`.

<details>
<summary>Comandos</summary>
{% highlight shell %}
#
# primeiro vamos modificar o script. A modificação permite que passemos parâmetro na execução do script.
$ touch curler.sh
$ echo "#!/bin/bash" > curler.sh
$ echo "echo 'Searching..'" >> curler.sh
$ echo "sleep 1" >> curler.sh
$ echo "curl http://$1" >> curler.sh
#
# criar o arquivo curler-dockerfile.
$ touch curler-dockerfile-v2
$ echo "FROM ubuntu:20.04" > curler-dockerfile-v2
$ echo "RUN apt update && apt upgrade -y && apt install curl -y" >> curler-dockerfile-v2
$ echo "COPY ./curler.sh" >> curler-dockerfile-v2
$ echo "RUN chmod +x curler.sh" >> curler-dockerfile-v2
$ echo 'ENTRYPOINT ["/curler.sh"]' >> curler-dockerfile-v2
#
# montar a imagem com a tag curler
$ docker build -t curler-v2 -f curler-dockerfile-v2 .
$ docker run curler-v2 google.com
{% endhighlight %}
</details><br>

### Exercício 11

Neste exercício, usaremos a ligação de volume para conectar um arquivo local com um arquivo no contêiner. A imagem utilizada para criação do contêiner é `devopsdockeruh/simple-web-service`. O arquivo no contêiner é gerado como um `log`. Deve-se utilizar a *flag* `-v`.

<details>
<summary>Comandos</summary>
{% highlight shell %}
#
# no diretório corrente vamos criar um arquivo
$ touch text.log
# agora vamos rodar um contêiner
$ docker run -v "$(pwd)/text.log:/usr/src/app/text.log" devopsdockeruh/simple-web-service
{% endhighlight %}
</details><br>

### Exercício 12

Neste exercício, usaremos a ligação de portas para conectar o host ao servidor web no contêiner. A imagem que deve ser utilizada é `devopsdockeruh/simple-web-service`. Nessa imagem, quando o comando `server` é passado para o comando `docker run` um servidor web é inicializado na porta 8080. No host, a porta de ligação fica a sua escolha.

<details>
<summary>Comandos</summary>
{% highlight shell %}
#
# comando básico
$ docker run -p8082:8080 devopsdockeruh/simple-web-service server
{% endhighlight %}
</details><br>

### Exercício 13

Neste exercício usaremos a aplicação [spring-example-project](https://github.com/pFransozi/docker-hy-material-applications/tree/main/spring-example-project) e a colocaremos em um contêiner que use a imagem `openjdk:_tag_`. Um `dockerfile` deve ser criado que copie os arquivos fontes da aplicação para a imagem e compile-os na imagem. Ao iniciar um contêiner baseado nessa imagem, deve ser possível passar o comando `java -jar ./target/docker-example-1.1.3.jar` ao `docker run` para iniciar a aplicação. Essa aplicação deve ser acessada pelo host usando uma ligação de porta.

<details>
<summary>Comandos</summary>
{% highlight shell %}
#
# primeiro deve-se clonar o projeto `spring-example-project` localmente.
$ git clone git@github.com:pFransozi/docker-hy-material-applications.git
#
# acessar o diretório da aplicação
$ cd spring-example-project
#
# criar um `dockerfile`
$ touch spring-example-project-dockerfile
$ echo "FROM openjdk:21-jdk-oraclelinux8" > spring-example-project-dockerfile
$ echo "COPY . /usr/src/myapp" >> spring-example-project-dockerfile
$ echo "WORKDIR /usr/src/myapp" >> spring-example-project-dockerfile
# compila a aplicação e a disponibiliza compilada na imagem.
$ echo "RUN ./mvnw package" >> spring-example-project-dockerfile
# com isso, podemos iniciar um contêiner já rodando a aplicação
$ echo 'ENTRYPOINT ["java"]' >> spring-example-project-dockerfile
#
# compilar uma imagem
$ docker build -t app-spring:v1 -f spring-example-project-dockerfile .
$ docker run -it -p8082:8080 app-spring:v1 -jar ./target/docker-example-1.1.3.jar
{% endhighlight %}
</details><br>

### Exercício 14

Criar uma imagem baseada na imagem `node:16.16.0-alpine3.16` que atenda os requisitos [deste projeto](https://github.com/pFransozi/docker-hy-material-applications/tree/main/example-frontend).

<details>
<summary>Comandos</summary>
{% highlight shell %}
# primeiro deve-se clonar o projeto `spring-example-project` localmente.
$ git clone git@github.com:pFransozi/docker-hy-material-applications.git
#
# acessar o diretório do arquivo fonte.
$ cd example-frontend
#
# criar arquivo `dockerfile`
$ touch ex-frontend-dockerfile
$ echo "FROM node:16.16.0-alpine3.16" > ex-frontend-dockerfile
$ echo "COPY . /usr/src/myapp" >> ex-frontend-dockerfile
$ echo "WORKDIR /usr/src/myapp" >> ex-frontend-dockerfile
$ echo "RUN npm install package.json" >> ex-frontend-dockerfile
$ echo "RUN npm run build" >> ex-frontend-dockerfile
$ echo "RUN npm install -g serve" >> ex-frontend-dockerfile
#
#
$ docker build -t ex-frontend:v1 -f ex-frontend-dockerfile .
#
#
docker run -it -p5001:5000 ex-frontend:v1 serve -s -l 5000 build
{% endhighlight %}
</details><br>

### Exercício 15

Criar uma imagem a partir da imagem `golang` que atenda os requisitos do [projeto](https://github.com/pFransozi/docker-hy-material-applications/tree/main/example-backend).

<details>
<summary>Comandos</summary>
{% highlight shell %}
# primeiro deve-se clonar o projeto `example-backend` localmente.
$ git clone git@github.com:pFransozi/docker-hy-material-applications.git
#
# acessar o diretório do arquivo fonte
$ cd example-backend
#
# criar arquivo `dockerfile`.
$ touch ex-backend-dockerfile
$ echo "FROM golang " > ex-backend-dockerfile
$ echo "COPY . /go/src/ " >> ex-backend-dockerfile
$ echo "WORKDIR /go/src/ " >> ex-backend-dockerfile
$ echo "RUN go build " >> ex-backend-dockerfile
$ echo "RUN go test " >> ex-backend-dockerfile
#
#
$ docker build -t go-app:v1 -f ./ex-backend-dockerfile .
$ docker run -it -p8083:8080 go-app:v1 ./server
{% endhighlight %}
</details><br>

[⇡](#introdução)

## Referências

[hub.docker.com](hub.docker.com){:target="_blank"}.

[docs.docker.com](docs.docker.com){:target="_blank"}.

[docs.docker.com: docker-hub official_images](https://docs.docker.com/docker-hub/official_images/){:target="_blank"}.

[docs.docker.com: commandline cli](https://docs.docker.com/engine/reference/commandline/cli/){:target="_blank"}.

[docs.docker.com: reference builder](https://docs.docker.com/engine/reference/builder/){:target="_blank"}.

[training.play-with-docker.com](https://training.play-with-docker.com/){:target="_blank"}.

[github.com: docker-library](https://github.com/docker-library){:target="_blank"}.

[github.com: official-images](https://github.com/docker-library/official-images){:target="_blank"}.

[github.com: wagoodman dive](https://github.com/wagoodman/dive){:target="_blank"}.

[en.wikipedia.org: docker](https://en.wikipedia.org/wiki/Docker_(software)){:target="_blank"}.

[mooc.fi](https://www.mooc.fi/en/){:target="_blank"}.

[devopswithdocker](https://devopswithdocker.com/){:target="_blank"}.

[github.com: docker-hy](https://github.com/docker-hy){:target="_blank"}.

[why-containers-stop](https://www.tutorialworks.com/why-containers-stop/){:target="_blank"}.

[docker-run-interactive-tty-options](https://www.baeldung.com/linux/docker-run-interactive-tty-options){:target="_blank"}.

[dockerfile-env-variable](https://www.baeldung.com/ops/dockerfile-env-variable){:target="_blank"}.

[what-does-the-input-device-is-not-a-tty-exactly-mean-in-docker-run-output](https://serverfault.com/questions/897847/what-does-the-input-device-is-not-a-tty-exactly-mean-in-docker-run-output){:target="_blank"}.

[quay.io](https://quay.io/){:target="_blank"}.

[docker-workdir-create-a-layer](https://stackoverflow.com/questions/71853912/docker-workdir-create-a-layer){:target="_blank"}.

[understanding-docker-without-losing-your-shit-cf2b30307c63](https://blog.hipolabs.com/understanding-docker-without-losing-your-shit-cf2b30307c63){:target="_blank"}.

[understanding-the-dockerfile-format-3cc6](https://dev.to/aws-builders/understanding-the-dockerfile-format-3cc6){:target="_blank"}.

[⇡](#introdução)