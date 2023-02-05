---
layout: post
title:  docker - uma introdução didática - parte 2
date:   2023-01-28 14:30:00
description: introdução à conteinerização com docker e docker compose, conceitos básicos, comandos e exemplos
tags: ["docker", "docker-compose", "containerization"]
language: pt-br
---
## Introdução

Esta é a segunda parte de um projeto de introdução ao *docker* e *docker compose*. Na primeira parte, exploramos conceitos de conteinerização e funcionalidades do *docker* em contêineres sem orquestração, tais como: criar arquivos de instruções, compilar images, diferentes maneiras de executar um contêiner, de encerrá-lo ou interagir com ele.

Agora, avançaremos um pouco na complexidade e incluiremos a orquestração no assunto. A seguir, o indíce de tópicos desta parte e links para as parte 1 e 2.
os tópicos abordados nesta parte e o link para as outras duas.

* [Parte 1]({% post_url 2023-01-28-docker-uma-introducao-p1 %}){:target="_blank"};
* [Parte 2](#parte-2)
  * [Começando com docker compose](#começando-com-docker-compose);
  * [Volumes com docker compose](#volumes-com-docker-compose);
  * [Web services com docker compose](#web-services-com-docker-compose);
  * [Redes no docker](#docker-networking);
  * [Dimensionamento (Scaling)](#dimensionamento-scaling);
  * [Volumes na prática](#volumes-na-prática);
  * [Contêineres para desenvolvimento](#contêineres-para-desenvolvimento)
  * [Exercícios](#exercícios)
  * [Referências](#referências)
* [Parte 3]({% post_url 2023-01-28-docker-uma-introducao-p3 %}){:target="_blank"}

## Parte 2

### Começando com docker compose

Em algumas situações, apenas um contêiner não é suficiente para atender a finalidade desejada, seja porque necessita-se de vários serviços que executam em distintos contêineres, seja porque necessita-se dimensionar algum serviço aumentando ou diminuindo sua disponibilidade. Nesse sentido, *docker compose* surge como uma ferramenta para definir e executar múltiplas aplicações em *docker* contêiner. Ele oferece algumas características:

* permitir uma composição complexa de múltiplos contêineres em um único *host*;
* isolamento das cargas de trabalho pelo *docker*;
* reutilização de configuração de ambientes (*IaC*);

Para testar se o *docker compose* está instalado corretamente, use o comando `docker compose version`. O resultado tem que ser `Docker Compose version v2.2.3`. `v2` é a última versão do *docker compose*, que vem em substituição a versão `v1`. Geralmente a `v1` ainda está disponível pelo alias `docker-compose`.

~~~ shell
$ docker-compose version
docker-compose version 1.25.0, build unknown
docker-py version: 4.1.0
CPython version: 3.8.10
OpenSSL version: OpenSSL 1.1.1f  31 Mar 2020
~~~

Enquanto a `v1` foi escrita em *python*, a `v2` foi em *go*, se incorporando ao ecossistema do *docker* em *go*. Mais informações sobre as diferenças entre as versões podem ser encontradas [aqui](https://www.docker.com/blog/announcing-compose-v2-general-availability/). Se por ventura `docker compose` não estiver disponível, siga as instruções abaixo para instalá-lo.

~~~ shell
$ mkdir -p ~/.docker/cli-plugins/
$ curl -SL https://github.com/docker/compose/releases/download/v2.2.3/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose
$ chmod +x ~/.docker/cli-plugins/docker-compose
$ docker compose version
Docker Compose version v2.2.3
~~~

Com o docker *compose instalado*, iniciemos por um exercício com o objetivo de executar um contêiner com um servidor web, usando o comando `docker run --rm -p 8080:80 --name nginx-compose nginx`. Nele usa-se uma imagem oficial `nginx`,  atribui-se um nome à instância do contêiner `nginx-compose`, faz-se a ligação das portas `8080` no *host*, `80` no contêiner, e ativa-se a *flag* `--rm`, que cumpre a função de remover o contêiner assim que ele encerrar a sua execução. Como não foi utilizada a *flag* `-d`, ao iniciar o contêiner o stream de saída começa a imprimir na tela informações sobre o serviço web.

~~~ log
2023/01/29 00:58:56 [notice] 1#1: using the "epoll" event method
2023/01/29 00:58:56 [notice] 1#1: nginx/1.23.3
2023/01/29 00:58:56 [notice] 1#1: built by gcc 10.2.1 20210110 (Debian 10.2.1-6) 
2023/01/29 00:58:56 [notice] 1#1: OS: Linux 5.14.0-1056-oem
2023/01/29 00:58:56 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2023/01/29 00:58:56 [notice] 1#1: start worker processes
2023/01/29 00:58:56 [notice] 1#1: start worker process 29
~~~

E para validar se o serviço foi iniciado corretamente, podemos executar uma requisição `http` ao servidor. Lembremos que do lado do *host*, o acesso é pela porta `8080`, que está ligada com a porta `80` do contêiner.

~~~ shell
$ curl 127.0.0.1:8080
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
~~~

Além disso, podemos incluir uma página no nosso servidor. Criando, por exemplo, um *hello world* em html.

~~~ shell
~/hello-world-page$ touch hello-world.html
~/hello-world-page$ echo "<!DOCTYPE html>" > hello-world.html
~/hello-world-page$ echo "<html>" >> hello-world.html
~/hello-world-page$ echo "<head>" >> hello-world.html
~/hello-world-page$ echo "<title>Hello World</title>" >> hello-world.html
~/hello-world-page$ echo "</head>" >> hello-world.html
~/hello-world-page$ echo "<body>" >> hello-world.html
~/hello-world-page$ echo "<p>Hi! This application should run on docker-compose</p>" >> hello-world.html
~/hello-world-page$ echo "</body>" >> hello-world.html
~/hello-world-page$ echo "</html>" >> hello-world.html
~~~

Em seguida, executando o comando `docker run --rm -p 8080:80 --name nginx-compose -v $(pwd)/static-site:/usr/share/nginx/html nginx`, pelo qual um diretório no contêiner é mapeado com um diretório no *host*, permitindo, assim, com que arquivos sejam compartilhadas e editados, neste caso. Em outros situações, pode-se negar a permissão de edição dos arquivos. No *host*, no diretório atual, `$(pwd)`, em que o comando foi executado, `hello-world-page`, caso não exista, será criado um diretório chamado `static-site`. Do outro lado, `static-site` é o diretório que o servidor do *nginx* consulta para os conteúdos estáticos. Portanto, precisamos copiar o arquivo `hello-world.html` para lá e assim: `curl 127.0.0.1:8080/hello-world.html`.

~~~ shell
~/hello-world-page/static-site$ curl 127.0.0.1:8080/hello-world.html
<!DOCTYPE html>
<html>
    <head>
        <title>Hello World</title>
    </head>
    <body>
       <p>Hi! This application should run on docker-compose</p>
    </body>
</html>
~~~ 

Com todos as configurações necessárias para executar um contêiner como planejado, podemos adaptar essas configurações para um arquivo de instruções do *docker compose*, conhecido como `docker-compose.yml`. Primeiro vamos criar o arquivo e imprimir nele as configurações. Em seguinda, vamos executar o comando `docker compose up` no mesmo diretório onde o arquivo foi criado. *Docker compose* busca por padrão o arquivo `docker-compose.yml`, porém é possível informar outro arquivo usando a flag `-f`, por exemplo `docker compose -f arquivo-alternativo.yml up`

~~~ shell
~/hello-world-page$ touch docker-compose.yml
~/hello-world-page$ echo \
"services:
  nginx:
    image: nginx
    ports:
      - 8080:80
    volumes:
      - ./static-site:/usr/share/nginx/html" > docker-compose.yml
~~~

~~~ shell
~/hello-world-page$ docker compose up
[+] Running 2/0
 ⠿ Network hello-world-page_default    Created                                                                                              0.0s
 ⠿ Container hello-world-page-nginx-1  Created                                                                                              0.0s
Attaching to hello-world-page-nginx-1
hello-world-page-nginx-1  | /docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
hello-world-page-nginx-1  | /docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
hello-world-page-nginx-1  | /docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
hello-world-page-nginx-1  | 10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
hello-world-page-nginx-1  | 10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
hello-world-page-nginx-1  | /docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
hello-world-page-nginx-1  | /docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
hello-world-page-nginx-1  | /docker-entrypoint.sh: Configuration complete; ready for start up
hello-world-page-nginx-1  | 2023/01/29 01:36:01 [notice] 1#1: using the "epoll" event method
hello-world-page-nginx-1  | 2023/01/29 01:36:01 [notice] 1#1: nginx/1.23.3
~~~

E agora o serviço está rodando pelo *docker compose*.

~~~ shell
$ curl 127.0.0.1:8080/hello-world.html
<!DOCTYPE html>
<html>
    <head>
        <title>Hello World</title>
    </head>
    <body>
       <p>Hi! This application should run on docker-compose</p>
    </body>
</html>
~~~

O servidor que executamos usa uma imagem padrão do *nginx*, mas é possível também construir uma imagem personalizada e utilizá-la na base da geração de contêineres. Primeiro passo é definir qual modificação será feita, por exemplo: modificar o arquivo de configurações do nginx para que o log seja gerado como json, pois permitirá utilizá-lo em ferramentas como *CloudWatch*, *StackDriver*, *ELK Stack*. Segundo, encontrar o contêiner que já está executando o nginx.

~~~ shell
~/hello-world-page$ docker ps
CONTAINER ID   IMAGE     COMMAND               CREATED            STATUS           PORTS                                  NAMES
af3fe9ffc3e1   nginx    "/docker-entrypoint.…" About an hour ago  Up About an hour 0.0.0.0:8080->80/tcp, :::8080->80/tcp  hello-world-page-nginx-1
~~~

Então, copiar o arquivo de configurações para servir de base, com o comando: `docker cp hello-world-page-nginx-1:/etc/nginx/nginx.conf nginx.conf`. Lembre-se que `hello-world-page-nginx-1` é o nome do contêiner e que o arquivo gerado, `nginx.conf`, será gerado no diretório atual onde o comando for executado. No caso deste exercício, consideramos `hello-world-page`.

~~~ shell
~/hello-world-page$ ls -l
docker-compose.yml  Dockerfile  nginx.conf  static-site
~~~

Modificando o arquivo `nginx.conf`, ao final deverá ficar como segue:

~~~shell
~/hello-world-page$ cat nginx.conf 
user nginx;
worker_processes auto;

error_log /var/log/nginx/error.log notice;
pid /var/run/nginx.pid;

events {
 worker_connections 1024;
}

http {
  include  /etc/nginx/mime.types;
  default_type  application/octet-stream;
  log_format  main  escape=json '{"remote_addr":"$remote_addr","remote_user":"$remote_user","time":"[$time_local]","request":"$request",'
                      '"status":"$status","body_bytes_sent":"$body_bytes_sent","http_referer":"$http_referer",'
                      '"http_user_agent":"$http_user_agent","http_x_forwarded_for":"$http_x_forwarded_for"}';
  access_log /var/log/nginx/access.log main;
  sendfile     on;
  #tcp_nopush  on;
  keepalive_timeout 65;
  #gzip  on;
  include /etc/nginx/conf.d/*.conf;
}
~~~

Com isso, criar um *dockerfile* e construir uma imagem:

~~~ shell
~/hello-world-page$ cat Dockerfile 
FROM nginx
COPY nginx.conf /etc/nginx/nginx.conf

~/hello-world-page$ docker build -t custom-nginx:0.1 .
~/hello-world-page$ docker images
REPOSITORY                               TAG        IMAGE ID       CREATED        SIZE
custom-nginx                             0.1        0647169b49d4   10 hours ago   142MB
~~~

Por fim, criar um `docker-compose.yml` com referência a imagem criada acima, e executá-la.

~~~ shell
~/hello-world-page$ echo \
"services:
  nginx:
    image: custom-nginx:0.1
    ports:
      - 8080:80
    volumes:
      - ./static-site:/usr/share/nginx/html" > docker-compose.yml

~/hello-world-page$ docker compose up
[+] Running 1/0
 ⠿ Container hello-world-page-nginx-1  Created                                                                                              0.0s
Attaching to hello-world-page-nginx-1
hello-world-page-nginx-1  | /docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
hello-world-page-nginx-1  | /docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
hello-world-page-nginx-1  | /docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
hello-world-page-nginx-1  | 10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
hello-world-page-nginx-1  | 10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
hello-world-page-nginx-1  | /docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
hello-world-page-nginx-1  | /docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
hello-world-page-nginx-1  | /docker-entrypoint.sh: Configuration complete; ready for start up
hello-world-page-nginx-1  | 2023/01/29 13:41:31 [notice] 1#1: using the "epoll" event method
hello-world-page-nginx-1  | 2023/01/29 13:41:31 [notice] 1#1: nginx/1.23.3
~~~

Esse foi um exemplo simples de como iniciar uma aplicação utilizando *docker compose*, ainda sem usarmos todo o potencial da ferramente: ambientes complexos e dimensionamento.  Antes de avançarmos em direção a isso, vamos retomar dois aspectos da arquitetura do *docker* e *docker compose*, a saber, volumes e redes.

[⇡](#introdução)

### Volumes com docker compose

Volume é a maneira pela qual *docker* e *docker compose* permitem com que sistemas de arquivos interajam, seja host com contêineres, seja contêineres com contêineres, e a maneira recomendada para persistir dados. Lembremo-nos que os dados dentro de um contêiner são efêmeros, isto quer dizer que toda fez que ele é destruído e recriado, todos os arquivos de configurações, por exemplo, de uma aplicação ou de um servidor web serão destruídos. Nesse sentido, é importante que dados que tenham um tempo de vida mais longo sejam persistidos fora dos contêineres, para isso, usa-se um volume.

Os volumes tem as seguintes características:

* dados armezanados nos volumes são persistentes;
* volumes não estão ligados a um único contêiner, mas podem ser vinculados a distintos;
* volumes podem ser ligados a mais de um contêiner ao mesmo tempo;
* volumes podem ser locais ou remotos;
* volumes são abstrações, isto é, não estão ligados diretamente ao sistema de arquivos locais, mas são gerenciados pelo *docker*.

Já vimos uma maneira simples de vincular um diretório externo a um diretório interno no container (dir-host:dir-contêiner) utilizando a *flag* `-v` tal como `-v $(pwd)/static-site:/usr/share/nginx/html` ou no *docker-compose*

~~~yml
volumes:
      - ./static-site:/usr/share/nginx/html
~~~

Com isso o diretório `/static-site` compartilha espaço com o diretório `/usr/share/nginx/html` e outros contêineres podem se ligar ao mesmo diretório no host. No entanto, nesse caso de uso há uma dependência à estrutura do sistema de arquivos e ao sistema operacional. Outra maneira é criar um volume usando `docker volume create exemplo-de-volume`, pelo qual atingimos um grau de abstração em relação à estrutura do sistema de arquivos e ao sistema operacional. Ao fim do trecho a seguir, fazer uma inspeção no volume `exemplo-de-volume` usando o camando `docker volume inspect`, pelo qual é exibido o seu arquivo de configurações. Dentre as configurações de um volume, temos `mountpoint` que indica onde no sistema de arquivos do *host* o volume está armazenado, embora seja o *docker* quem o gerencie.

~~~ shell
~/hello-world-page$ docker volume create exemplo-de-volume
exemplo-de-volume
~/hello-world-page$ docker volume ls
DRIVER    VOLUME NAME
local     exemplo-de-volume
~/hello-world-page$ docker volume inspect exemplo-de-volume
[
    {
        "CreatedAt": "2023-01-29T14:45:39-03:00",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/exemplo-de-volume/_data",
        "Name": "exemplo-de-volume",
        "Options": {},
        "Scope": "local"
    }
]
~~~

Agora jápodemos vincular o volume criado a um contêiner, usando `--mount`. Por um lado, indica-se a origem, que no nosso caso é o nome do volume `exemplo-de-volume`, por outro lado, o alvo, ou seja, em qual diretório o volume será montado no contêiner. Ao fim, temos `--mount source=exemplo-de-volume,target=/storage`. No exemplo a seguir, o contêiner executa um terminal bash, no qual um arquivo chamado `arquivo.teste` é criado no volume. Em seguida, um outro contêiner é gerado, vinculado ao mesmo volume e, como se espera, o arquivo deve ser compartilhado entre os contêineres.

* primeiro terminal bash
~~~ shell
~/hello-world-page$ docker run -it --rm --name exemplo-volume-container --mount source=exemplo-de-volume,target=/storage bash
bash-5.2# ls -l
total 60
drwxr-xr-x    2 root     root          4096 Jan 29 17:45 storage
bash-5.2# cd storage/
bash-5.2# pwd
/storage
bash-5.2# touch arquivo.teste
bash-5.2# ls -l
total 0
-rw-r--r--    1 root     root             0 Jan 29 18:03 arquivo.teste
bash-5.2# echo arquivo.teste > arquivo.teste 
bash-5.2# cat arquivo.teste 
arquivo.teste
bash-5.2# 
~~~
* segundo terminal bash
~~~ shell
docker run -it --rm --name exemplo-volume-container-dois --mount source=exemplo-de-volume,target=/storage bash
bash-5.2# 
bash-5.2# cd storage/
bash-5.2# pwd
/storage
bash-5.2# ls -l
total 4
-rw-r--r--    1 root     root            14 Jan 29 18:03 arquivo.teste
bash-5.2# cat arquivo.teste 
arquivo.teste
~~~

Em outro teste, vamos executar dois contêineres acessando o mesmo volume de modo tal que durante certo tempo eles acessem simultaneamente o mesmo volume. O teste é simples, mas atinge o objetivo de mostrar que o mesmo volume não fica restrito ao contêiner que o acesse primeiro.

~~~ shell
~/hello-world-page$ docker run -d -it --name container-2 --mount source=exemplo-de-volume,target=/storage bash -c "for i in \$(seq 1 1 100000); do echo \$HOSTNAME \$i >> /storage/\$HOSTNAME.txt; done"

~/hello-world-page$ docker run -d -it --name container-1 --mount source=exemplo-de-volume,target=/storage bash -c "for i in \$(seq 1 1 100000); do echo \$HOSTNAME \$i >> /storage/\$HOSTNAME.txt; done"

~/hello-world-page$ docker run -it --rm --name container-3 --mount source=exemplo-de-volume,target=/storage bash
bash-5.2# cd storage/
bash-5.2# pwd
/storage
bash-5.2# ls -l
total 3700
-rw-r--r--    1 root     root            21 Jan 29 18:12 arquivo.teste
-rw-r--r--    1 root     root       1888895 Jan 29 18:17 bb6ecfe97f74.txt
-rw-r--r--    1 root     root       1888895 Jan 29 18:17 d8515eddad7e.txt
bash-5.2# 
~~~

Como esperado, os arquivos gerados pelos dois contêineres estão presentes no volume. Alguns cenários podem exigir que um contêiner interaja com um volume escrevendo e lendo, mas em outros cenários pode ser mais seguro que o contêiner apenas lei os arquivos no volume.

~~~ shell
~/Documents/docker-parte-2$ docker run -it --rm --name conteiner-volume-exemplo --mount source=exemplo-de-volume,target=/storage,readonly bash
bash-5.2# cd storage/
bash-5.2# pwd
/storage
bash-5.2# ls -l
total 3700
-rw-r--r--    1 root     root            21 Jan 29 18:12 arquivo.teste
-rw-r--r--    1 root     root       1888895 Jan 29 18:17 bb6ecfe97f74.txt
-rw-r--r--    1 root     root       1888895 Jan 29 18:17 d8515eddad7e.txt
bash-5.2# touch ops
touch: ops: Read-only file system
bash-5.2# 
~~~

Quando falamos de *docker-compose*, a configuração de volumes é tão simples como visto até aqui.

~~~ yml
services:
  nginx:
    image: nginx
volumes:
  exemplo-de-volume:
~~~

~~~ yml
services:
volumes:
  exemplo-de-volume-nfs:
    driver_opts:
      type: "nfs"
      o: "addr=127.0.0.1,nolock,rw"
      device: ":/data"
~~~

*NFS* significa *network file system*, utilizado para conectar em repositório na rede.

### Web services com docker compose

Vamos testar um exemplo de web service simples. `jwilder/whoami` é um simples serviço http no docker que imprime o id do contêiner.

~~~ shell
~$ docker container run -d -p 8000:8000 jwilder/whoami
972bc82fd97d68e6dbd7278fa2b6b2a5a47d97a5bb2f92d8a9378df5afb9e450
~$ curl localhost:8000
I'm 972bc82fd97d
~~~

Agora vamos usar docker-compose com esse serviço.

~~~ yml
version: '3.8'

services:
    whoami:
      image: jwilder/whoami
      ports:
        - 8000:8000
~~~

Criei o arquivo acima como `docker-compose-yml` e no mesmo diretório execute o comando `docker compose up -d`. Lembre-se de encerrar o serviço que iniciamos no passo anterior na porta 8000, caso não o faça, *docker compose* não conseguirá iniciar a configuração acima.

~~~ shell
~/$ docker compose up -d
[+] Running 1/1
 ⠿ Container documents-whoami-1  Started                                                                                                    0.4s
~/$ curl localhost:8000
I'm 0ff639c2ee14
~~~

### Redes no docker

Rede é uma das principais funcionalidades do *docker*, através do qual contêineres comunicam e interagem enrte si de modo transparente. Algumas características dessa funcionalidade são:

* redes privadas;
* *dns* interno;
* comunicação entre contêineres;
* *bridging* entre contêineres;

No *docker* existem três tipos de redes básicas:

* *bridge* é o tipo padrão de rede quando se cria um contêiner. Esse tipo consiste de uma camada lógica, software, que prove conectividade a todos os contêineres conectados, e os isola de contêineres que estão em outra camada de rede.
~~~ shell
~/Documents/docker-parte-2$ docker run --rm -p 8080:80 --name nginx-compose nginx
2023/01/29 19:25:26 [notice] 1#1: using the "epoll" event method
2023/01/29 19:25:26 [notice] 1#1: nginx/1.23.3
2023/01/29 19:25:26 [notice] 1#1: built by gcc 10.2.1 20210110 (Debian 10.2.1-6) 
#
# em outro terminal
~/Documents/docker-parte-2$ docker inspect --format '{{json .NetworkSettings.Networks }}' nginx-compose
{"bridge":{"IPAMConfig":null,"Links":null,"Aliases":null,"NetworkID":"40219a49a8a2b5f44d0830f04aaae6994a93790081235333861f0fc043819552","EndpointID":"c7b5a7a0b10ffd7ea61d420808520f9d8495cee4041eff2c1d31eca44421f2b1","Gateway":"172.17.0.1","IPAddress":"172.17.0.3","IPPrefixLen":16,"IPv6Gateway":"","GlobalIPv6Address":"","GlobalIPv6PrefixLen":0,"MacAddress":"02:42:ac:11:00:03","DriverOpts":null}}
~/Documents/docker-parte-2$ docker network ls
NETWORK ID     NAME                       DRIVER    SCOPE
40219a49a8a2   bridge                     bridge    local
57c93e712bce   host                       host      local
72eda63a81bb   none                       null      local
~~~
O contêiner que acabamos de executar foi vinculado à rede bridge padrão `"NetworkID":"40219a49a8a2b5f44d0830f04aaae6994a93790081235333861f0fc043819552"` e `40219a49a8a2   bridge                     bridge    local`.
* *host* é um tipo de rede que não concede um ip específico para contêiner, mas compartilha o mesmo do host. Nesse caso, se iniciamos um servidor nginx, ele não será ligado com um host por uma porta (8080:80), mas ele fica acessível diretamente por localhost:80.
~~~ shell
~$ docker run --rm --network host --name host-nginx nginx
2023/01/29 19:34:58 [notice] 1#1: using the "epoll" event method
2023/01/29 19:34:58 [notice] 1#1: nginx/1.23.3
2023/01/29 19:34:58 [notice] 1#1: built by gcc 10.2.1 20210110 (Debian 10.2.1-6) 
2023/01/29 19:34:58 [notice] 1#1: OS: Linux 5.14.0-1056-oem
2023/01/29 19:34:58 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2023/01/29 19:34:58 [notice] 1#1: start worker processes
#
# em outro terminal, podemos acessar
~$ curl localhost
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
~~~
* *overlay* é um tipo de rede que permite conectar vários diferentes hosts, em um orquestração de hosts que suportam os contêineres do docker. Técnica muito comum em ambientes de produção, nos quais demandas muito maiores de dimensionamento de recursos podem ser exigidos. *Overlay* cria uma camada lógica de rede distribuída entre os hosts, de modo transparente conecta as redes dos hosts e cria uma rede unificada sobre elas.

Olhando para o *docker-compose*, de modo similar a contêineres isolados, quando iniciados pelo *docker-compose* sem especificar um tipo de rede, por padrão eles utilizarão modo *bridge*.

~~~ yml
services:
  servico-no-conteiner:
    build:
      context: 
      labels:
        - com.packtpub.compose.app=servico-no-conteiner
      image: servico-no-conteiner:0.1
      networks:
        - servico-no-conteiner-pubic-network
      labels:
        - com.packtpub.compose.app=servico-no-conteiner
  redis:
    image: redis
    networks:
      - servico-no-conteiner-pubic-network  
    labels:
      - com.packtpub.compose.app=servico-no-conteiner
  redis-backup:
    image: bash
    networks:
      - servico-no-conteiner-pubic-network
    labels:
      - com.packtpub.compose.app=servico-no-conteiner
networks:
  servico-no-conteiner-pubic-network:
    labels:
      - com.packtpub.compose.app=servico-no-conteiner
~~~

~~~ shell
~$ docker compose up
~$ docker network ls
NETWORK ID     NAME                                  DRIVER    SCOPE
ce5aa144f0f9   servico-no-conteiner-pubic-network    bridge    local
~~~

Com isso, todos os serviços se comunicam pela mesma rede `servico-no-conteiner-pubic-network`.

### Dimensionamento (Scaling)

Retomando ao exemplo da imagem `jwilder/whoami`, vamos falar um pouco sobre redimensionamento. Estamos rodando o serviço `whoami` em apenas um contêiner.

~~~ shell
~/$ docker compose ps
NAME                 COMMAND             SERVICE             STATUS              PORTS
whoami-1   "/app/http"         whoami              running             0.0.0.0:8000->8000/tcp, :::8000->8000/tcp
~~~

*Docker* e *docker compose* possuem um rico conjunto de manuais de ajuda. Por exemplo, `docker compose up --help` me apresenta todas as `flags` que podem ser combinadas com `docker compose up`. Dentre essas encontramos `--scale scale` que é descrito como: "dimensiona um serviço para um número específico de instâncias. Caso executado, sobreescreve o número de instâncias dimensionado no arquivo docker-compose-yml". Manuais de comandos são sempre interessantes porque em muitos casos nos dizem um pouco mais do que estavamos procurando. Procurávamos um comando para dimensionamento do serviço e foi confirmado pelo manual do `scale`. Ademais, aprendemos que o dimensionamento pode ser pre-definido no arquivo de intruções do docker-compose.

Vamos executar `scale` na nossa instância do serviço `whoami`. Observe que o serviço é indicado no comando `whoami=5`. Acaso o serviço for *nginx*, `nginx=5`.

~~~ shell
~/$ docker compose up --scale whoami=5
[+] Running 5/5
 ⠿ Container documents-whoami-5  Created                                                                                                    0.1s
 ⠿ Container documents-whoami-2  Created                                                                                                    0.1s
 ⠿ Container documents-whoami-4  Created                                                                                                    0.2s
 ⠿ Container documents-whoami-1  Recreated                                                                                                  0.4s
 ⠿ Container documents-whoami-3  Created                                                                                                    0.1s
Attaching to documents-whoami-1, documents-whoami-2, documents-whoami-3, documents-whoami-4, documents-whoami-5
Error response from daemon: driver failed programming external connectivity on endpoint documents-whoami-1 (3341c31a4fc402862d825e10015505005759ba47955d3b2f1ff6c6d26309279e): Bind for 0.0.0.0:8000 failed: port is already allocated
~~~

No entendo a execução foi não sucedida como esperávamos. Ao fim, recebemos uma mensagem de erro. Isso ocorreu porque definimos uma porta fixa no arquivo de instruções: `Bind for 0.0.0.0:8000 failed: port is already allocated`.

~~~ yml
ports:
        - 8000:8000
~~~

Determinar uma porta fixa ocasionou um conflito a partir do primeiro contêiner gerado, pois o *docker* tentou ligar todos os demais à porta *8000* do host, produzindo uma exceção. Uma maneira de contornar essa exceção é não vincular uma porta fixa no host, atribuindo apenas a porta do serviço no contêiner. Desse modo *docker* solicitará ao sistema operação uma porta dinâmica que está na faixa 49152 - 65535.

Mudando o arquivo de instruções do *docker compose* e executando-o.
~~~ yml
ports:
        - 8000
~~~

Executando um novo container com as novas configurações não será mais possível conectar usando a porta `8000`, mas `49153` foi selecionada pelo *docker*.
~~~ shell
~/$ docker compose up 
[+] Running 1/0
 ⠿ Container documents-whoami-1  Created                                                                                                    0.0s
Attaching to documents-whoami-1
documents-whoami-1  | Listening on :8000
documents-whoami-1  | I'm e6326b7328db

#
# em outro console
~/$ docker compose ps
NAME                 COMMAND             SERVICE             STATUS              PORTS
documents-whoami-1   "/app/http"         whoami              running             0.0.0.0:49153->8000/tcp, :::49153->8000/tcp
~/$ curl localhost:8000
curl: (7) Failed to connect to localhost port 8000: Connection refused
~/$ curl localhost:49153
I'm e6326b7328db
~/$ 
~~~

A versão básica funcionou, agora podemos escalar o serviço.

~~~ shell
~/$ docker compose up --scale whoami=5
[+] Running 5/5
 ⠿ Container documents-whoami-5  Created                                                                                                    0.0s
 ⠿ Container documents-whoami-2  Created                                                                                                    0.0s
 ⠿ Container documents-whoami-3  Created                                                                                                    0.0s
 ⠿ Container documents-whoami-1  Recreated                                                                                                  0.3s
 ⠿ Container documents-whoami-4  Created                                                                                                    0.0s
Attaching to documents-whoami-1, documents-whoami-2, documents-whoami-3, documents-whoami-4, documents-whoami-5
documents-whoami-3  | Listening on :8000
documents-whoami-4  | Listening on :8000
documents-whoami-2  | Listening on :8000
documents-whoami-5  | Listening on :8000
documents-whoami-1  | Listening on :8000
~~~
~~~ shell
~/$ docker compose ps
NAME                 COMMAND             SERVICE             STATUS              PORTS
documents-whoami-1   "/app/http"         whoami              running             0.0.0.0:49156->8000/tcp, :::49156->8000/tcp
documents-whoami-2   "/app/http"         whoami              running             0.0.0.0:49158->8000/tcp, :::49158->8000/tcp
documents-whoami-3   "/app/http"         whoami              running             0.0.0.0:49154->8000/tcp, :::49154->8000/tcp
documents-whoami-4   "/app/http"         whoami              running             0.0.0.0:49155->8000/tcp, :::49155->8000/tcp
documents-whoami-5   "/app/http"         whoami              running             0.0.0.0:49157->8000/tcp, :::49157->8000/tcp
~/$
~~~

No entanto essa solução ainda tem limitações. Geralmente, na frente do serviço dimensionável, seria executado um *load-balancer*, no qual as requisições se concentrariam e desde o qual elas seriam distribuídas para as várias instâncias que executam o serviço. Vamos ver um exemplo.

~~~ yml
~~~

### Volumes na prática

### Contêineres para desenvolvimento

### Exercícios

## Referências

https://www.docker.com/blog/announcing-compose-v2-general-availability/

https://stackoverflow.com/questions/36685980/why-is-docker-installed-but-not-docker-compose

https://docs.docker.com/compose/

https://www.docker.com/blog/how-to-use-the-official-nginx-docker-image/

https://stackoverflow.com/questions/25049667/how-to-generate-a-json-log-from-nginx

https://docs.docker.com/storage/volumes/

https://www.docker.com/blog/announcing-compose-v2-general-availability/

https://docs.docker.com/compose/reference/

https://docs.docker.com/storage/bind-mounts/