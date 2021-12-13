
# Bem Vindos ao acompanhamento do meu aprendizado na Wine!
 _Apenas a melhor empresa com o melhor time de DevOps que o BR já viu_

## Curso: Docker and Kubernetes: The Complete Guide
> Este documento irá conter todos os pontos mais importantes identificados por mim.
> A cada módulo, será montado uma "div" com os tópicos mais importantes de cada aula assistida.


## Dia 06 de dezembro de 2021 - Módulo 01 
### Dive into Docker!
> O que é o Docker? Basicamente, é um ecossistema de criação e chamada de contêineres.
  Um ecossistema Docker é formado tipicamente pelo Docker Client, Docker Server, Docker Machine, Docker Image, Docker Hub e Docker Compose.
  Para entendermos melhor o Docker é necessário nos familiarizarmos com dois termos vitais: **Imagem** e **Contêiner**.
* Imagem: Arquivo único com todas as dependências e configurações requeridas para rodar um programa. Uma imagem é formada, basicamente, pela *FileSystem Snapshot* e pelo *Startup Command* 
* Container: Instância da imagem, roda o programa em si. Um container é formado, basicamente, pelo *Running Process*, Kernel, RAM, CPU, Network e pelo segmento do disco rígido disponibilizado para tal processo. 

1. **Imagem**
    | FS Snapshot               | Startup Command                  |
    | ------------------------- | -------------------------------- |
    | bin, dev, etc, home, proc | {Algum comando de inicialização} |

2. **Container**
    Running Process -> Kernel -> Ram, Network, CPU -> HDD seg.

> Quando um container é instalado em uma máquina local, o *FS Snapshot* é armazenado no *HDD seg*, enquanto o *Startup Command* é direcionado para o *Running Process*.

#### Instalando o Docker no Linux
1. Caso tenha alguma versão antiga do Docker já instalada, remova-a usando
    > $  sudo apt-get remove docker docker-engine docker.io containerd runc

2. Configurando o repositório:
    > $ sudo apt-get update (para atualizar os pacotes na fila)
    > $ sudo apt-get install \
       ca-certificates \
       curl \
       gnupg \
       lsb-release

3. Adicionando a chave GPG oficial do Docker:
    > $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

4. O seguinte comando configura o repositório tipo **stable**
    > $ echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

5. Instalando Docker Engine
    ~~~
    $ sudo apt-get update
    $ sudo apt-get install docker-ce docker-ce-cli containerd.io
    ~~~

6. Ao fim, você pode rodar um programa _Hello World_ para verificar se a instalação ocorreu de forma correta!
    > $ sudo docker run hello-world 

#### Usando Docker Client; O que são Contêineres; Como o Docker funciona na sua máquina;
> Podemos verificar que, no momento que rodamos o hello-world, o Docker nos envia uma mensagem afirmando que não foi encontrado na máquina local a imagem requisitada. Portanto, o Docker Client (solicitante do hello-world) solicita ao Docker Server para que ele procure no Docker Hub se existe alguma imagem chamada hello-word que possa ser enviada e salva na máquina local.  

> Portanto, para finalizar a Seção:
  * Docker é um conjunto de produtos que entregam softwares em pacotes chamados *contêineres*.
  * Contêineres agrupam seus próprios softwares, bibliotecas, scripts, etc. São executados pelo kernel do SO e economiza recursos da máquina.

## Dia 07 de dezembro de 2021 - Módulo 2
### Manipulating Containers With the Docker Client
>  A chamada usual para qualquer função Docker é:
  `$ sudo docker run <image name>`
> *Ex: $ sudo docker run hello-world*

> utilizando a imagem do Busy Box, podemos nos aprofundar muito mais na usabilidade do Docker:
  `$ sudo docker run busybox echo aposta 10 no galo`
       *aposta 10 no galo*

> Nesse caso, temos o comando 'echo' passando um parâmetro para ser printado na tela. Nesse caso, isso se deve ao fato da imagem BusyBox possuir uma série de atributos que podem ser usados. 
Caso tentássemos usar o comando 'echo' no contêiner hello-world, por exemplo, o Docker retornaria um erro, pois a imagem hello-world não possui esse comando. 

> esses comandos são chamados de *running process*.

> **Palavras do dia:** *ping google.com, docker ps, docker ps --all*

> Quando usamos o comando docker run, significa que chamamos outros dois comandos: o docker create e o docker start

### Dia 08 de dezembro de 2021 - Módulo 2
#### Ciclo de Vida do Contêiner
> Ao chamar o comando docker ps --all, uma tabela de logs de chamados de contêineres(usos de docker run) é aberta. Um dos dados dessa tabela é o status de cada chamado, onde é exibida a seguinte mensagem:
  `Exited (0) X seconds/minutes/hours ago`
> Porém, também é exibido o ID do container. Com esse ID, podemos "ativar" novamente o uso do container. Chamado: `docker start -a <ID do container>`.

> Com o comando `docker system prune`, todos os containers pausados são apagados. É importante lembrar sempre que haver um acúmulo desnecessário de containers baixados em sua máquina.

> Caso queria visualizar o conteúdo de um container sem precisar reinicia-lo, posso usar o comando `docker logs <ID do container>`. É bem útil para debugs, por exemplo.

> Para finalizar um container que está rodando em segundo plano, ou em loop eterno, como no caso do comando ping, posso usar dois comandos: `docker stop` ou `docker kill`. O comando docker stop finaliza o container de forma paulatina, já o docker kill finaliza o container instantâneamente.

#### Multi-comandos em Containers
> Caso seja necessário, pode-se realizar mais de um comando por vez utilizando o Docker. Nesse caso, antes de mais nada, é preciso que um container esteja sendo executado. 
  * Pegue como exemplo o Redis, um software de armazenamento de estrutura de dados em memória. Ele possui seus próprios comandos e atributos. Caso queira iniciar um container do redis, basta que eu use `$ docker run redis`.
  * Entretando, caso eu queira utilizar um comando do Redis dentro de uma inicialização Docker, eu não tenho acesso a esse comando. Portanto, para que eu consiga acessar tal comando, preciso utilizar o docker exec
    `$ docker exec -it <id do container> <comando>`
  * Lembrando que o docker exec deve ser executado em uma nova janela do Terminal.
     - flag -i permite a entrada de dados;
     - flag -t formata/identa a entrada de dados;
  * O comando `sh` permite trabalhar em uma shell, para que não seja necessário repetir docker exec toda vez que for preciso executar um novo comando. 

**Palavras/frases do dia:** *docker system prune, docker stop, docker kill, docker exec -it, sh. Multi-comandos, Shell*

## Módulo 3 - Building Custom Images Through Docker Server

> Sabemos que todos os containers foram construídos por alguém. Portanto, também é extremamente válido aprender como construir nossos próprios containers. Basicamente, a construção de um container segue o seguinte fluxo:

  - Docker File --> Docker Client --> Docker Server --> Imagem 

#### Criando um Dockerfile
> O primeiro passo para a criação de uma Imagem Docker é construir seu Dockerfile. O Dockerfile é um arquivo sem extensão que possui algumas palavras reservadas. Como exemplo de criação de um Dockerfile clone do Redis-client, temos:

  | Instrução p/ Server Docker | Argumento da Instrução |
  | :------------------------: | :--------------------: |
  |            FROM            |         alpine         |
  |            RUN             | apk add --update redis |
  |            CMD             |    ["redis-server"]    |

> Alpine é uma imagem baseada na distribuição Linux de mesmo nome, que possui características como gestão de pacotes apk e execução sobre a RAM. 

> logo após a criação do Dockerfile, podemos executa-lo pelo terminal usando `$ docker build .`, dentro do diretório origem do arquivo. 

> Podemos reaproveitar um Dockerfile para acrescentar novos pacotes a serem instalados quando construímos a imagem pelo docker build. Pegue esse exemplo: 

  | Instrução p/ Server Docker | Argumento da Instrução |
  | :------------------------: | :--------------------: |
  |            FROM            |         alpine         |
  |            RUN             | apk add --update redis |
  |            RUN             |  apk add --update gcc  |
  |            CMD             |    ["redis-server"]    |

> Nesse caso, a Imagem "redis-server" terá o acréscimo do pacote gcc. Ao rodarmos o comando docker build, vemos que um novo passo(step) é adicionado, baixando e instalando os componentes do gcc. Entretanto, quando analizamos o log dos outros três passos que já existiam, a mensagem "Using Cache" aparece. Isso mostra que não foi reconstruídos os steps já existentes.
> Agora, caso invertamos a ordem de adição (RUN gcc primeiro e depois RUN redis), todos os steps serão reinstalados ao comando docker build.
> Moral da história: Caso queira reutilizar Dockerfiles, sempre coloque as mudanças nos finais dos arquivos ou o mais abaixo possível do código pré-existente.

> Para renomear a Imagem criada em sua máquina, podemos chamar o docker build com os seguintes parâmetros:
    `$ docker build -t <docker-id-do-user>/<nome_da_imagem>:latest .`
  

## Dia 09 de dezembro de 2021 - Módulo 4
### Making Real Projects with Docker

> Para darmos continuidade no aprendizado de Docker, iniciamos mais um projeto. Dessa vez, iremos construir uma aplicação web simples utilizando Node JS. 

* Primeiro criamos um *package.json*
    ~~~JSON
      {
      "dependencies": {
          "express": "*"
      },
      "scripts": {
          "start": "node index.js"
      }
     }
    ~~~

* Em seguida, criamos o *index.js*
    ~~~javascript
      const express = require('express');
        const app = express();

        app.get('/', (req, res) => {
            res.send('Hi there');
        });

        app.listen(8080, () =>{
            console.log('Listening on port 8080!');
        });
    ~~~

* Por último, o Dockerfile:
>
    ~~~dockerfile
      # Base image
      FROM node:alpine

      # Install dependencies
      WORKDIR /usr/app
      COPY ./ ./
      RUN npm install

      # Command default
      CMD ["npm", "start"]
    ~~~

> A dependência WORKDIR foi necessária para corrigir o erro `npm ERR! Tracker "idealTree" already exists`, criando um novo diretório aonde podemos trabalhar sem ter medo de sobrescrever algum arquivo base do container.
> A dependência COPY é necessária para transferir todos os arquivos que estão na pasta de trabalho para o container temporário, necessário para construir a Imagem.

> Após todos os arquivos estarem prontos, o comando *docker build* foi chamado e a Imagem foi construída sem maiores problemas.

#### Container Port Mapping

> Podemos perceber no *index.js* que o algoritmo em questão está criando uma conexão com a porta 8080 do nosso localhost. Entretanto, quando tentamos acessar essa porta pelo browser, após a imagem docker estar sendo gerada e rodando sem erros, percebemos que não conseguimos acessar mesmo assim.
> Isso acontece pois a porta 8080 local não está conversando com a porta 8080 do nosso container. Para resolver isso, fazemos um *Port Mapping*
  `$ docker run -p <porta/-local>:<porta_container> <nome ou id da imagem>`
  Exemplo: docker run -p 8080:8080 winerodmattiolli/simpleweb

> Feito isso, agora podemos acessar nossa aplicação web pelo browser, usando seu endereço de localhost. 

#### Minimizando rebuilds 

> Como sabemos, estamos construindo uma aplicação web e, como todo projeto, ele vai se desenvolvendo aos poucos. A cada atualização, precisamos salvar as mudanças. Porém quando usamos o Docker, as mudanças são atualizadas no container automaticamente? Claramente não é assim que funciona e isso pode gerar alguns problemas.
> Digamos que fazemos uma atualização no *index.js*. Ao salvarmos o arquivo novamente, notamos que nada acontece na porta 8080 do container. 
> Logo, percebemos que é necessário realizar um novo *docker run*. Mas isso não é interessante pois irá afetar todos os arquivos salvos no container, sendo que só modificamos um!
> Para resolvermos isso, podemos duplicar a dependência COPY, separando o *package.json* do resto dos arquivos.
> Isso agiliza muito nossos *reruns* e economiza recursos tanto do container quanto da máquina local.
> Portanto, nosso Dockerfile otimizado para reruns será:

    ~~~dockerfile
      # Base image
      FROM node:alpine

      # Install dependencies
      WORKDIR /usr/app
      COPY ./package.json ./
      RUN npm install
      COPY ./ ./

      # Command default
      CMD ["npm", "start"]
    ~~~

> Sendo assim, não precisamos mais nos preocupar tanto com a quantidade de atualizações que faremos dentro do projeto.

**Palavras/Frases do dia**:*Cache, Dockerfile, port mapping, minimizar rebuilds*

## Dia 13 de dezembro de 2021 - Módulo 5
### Docker Compose

> Pegue o seguinte exemplo: uma imagem é criada para construir uma aplicação web. Para realizar essa aplicação, são necessários dois containers rodando ao mesmo tempo e se comunicando. O Docker Compose permite a criação/chamada de múltiplos containers ao mesmo tempo, permitindo comunicação entre eles.

> Para isso, um novo tipo de arquivo deve ser criado, o `docker-compose.yml`.

> o arquivo docker-compose será responsável por inicializar todos os containers envolvidos ao mesmo tempo e criar conexões entre eles. Sua sintaxe será basicamente:
    ~~~yml
      version: '3' # versão padrão
      services:
        redis-server: # primeiro container a ser chamado
          image: 'redis' # exemplo de imagem a ser chamada
        node-app: # segundo container a ser chamado
          build: .
          ports:
            - "4001:8081" # porta local conectada à porta do container
    ~~~

> O comando inicial do docker-compose é `docker-compose up`, podendo haver a flag `--build` caso algum container tenha sofrido alterações e é necessário um rebuild.

> O docker-compose também possui a possibilidade de rodar pelo background. Para isso, basta usar `docker-compose up -d`.

> Caso queira parar um docker-compose, basta utilizar o comando `docker-compose down`. Esse comando irá parar e remover todos os containers iniciados pelo docker-compose. 

> Podemos usar ainda o comando `docker-compose ps` para visualizar o status de todos os containers que existem dentro do docker-compose. 

#### Políticas de Restart
> Dentro do arquivo YML do docker-compose, existe uma tag de tratamento de crash de container, caso seja necessário usar. As regras são as seguintes:
    
    "no" (sempre entre aspas) | nunca tentar reiniciar o container se ele parar ou crashar
    always | sempre tentar reiniciar o container se ele parar por qualquer motivo
    on-failure | reiniciar apenas se o container parar por algum código de erro
    unless-stopped | sempre reiniciar a menos que o desenvolvedor para-lo de forma forçada