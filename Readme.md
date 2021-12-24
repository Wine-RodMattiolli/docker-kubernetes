
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
>    
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
    
   |       Terminologia        |                              Significado                              |
   | :-----------------------: | :-------------------------------------------------------------------: |
   | "no" (sempre entre aspas) |      nunca tentar reiniciar o container se ele parar ou crashar       |
   |          always           | sempre tentar reiniciar o container se ele parar por qualquer motivo  |
   |        on-failure         |    reiniciar apenas se o container parar por algum código de erro     |
   |      unless-stopped       | sempre reiniciar a menos que o desenvolvedor para-lo de forma forçada |

## Dia 15 de dezembro de 2021 - Módulo 6
### Creating a Production-Grade Workflow

> Nessa seção, é apresentado um dos fluxos de trabalho (workflow) mais usuais do momento.

> Esse Workflow consiste no seguinte: `Development --> Testing --> Deployment`. Basicamente, desenvolver a aplicação em máquina local, testar o código e depois realizar o deploy. Porém várias sub-etapas são necessárias para manter um bom workflow.

1. Development
    * Dev cria e modifica a branch feature;
    * Faz mudanças em quaisquer outras branches que não são Master;
    * Push para o GitHub;
    * Cria Pull Request para realizar merge com a branch Master.
  
2. Testing
    * Push do código da branch master para o **Travis CI**;
      > Travis CI é um Provedor de Integração Contínua, é nele que iremos rodar vários testes.
    * Roda os testes;
    * Se tudo estiver OK, realiza merge com todas as mudanças necessárias que os testes geraram para a branch master.
  
3. Production/Deployment
   * Push do código do Travis CI;
   * Roda outros testes, caso haja necessidade;
   * Realiza o Deploy para a AWS, especificamente para o AWS Elastic Beanstalk

#### Aonde entra o Docker nessa brincadeira?
> Percebemos que, na lista de passos acima, não é citado em momento algum o Docker. Bom, ele não é mesmo um requisito para que possamos fazer todas as tarefas do Workflow, porém é uma ferramenta poderosíssima para facilitar alguns passos do development. 

#### Docker Volumes
> Nessa seção, treinamos o workflow em uma aplicação React.JS + Docker. Dessa forma, vemos que existe uma dificuldade de fazer a atualização em tempo real das mudanças realizadas dentro da aplicação react. Sendo assim, o conceito de Docker Volume é aplicado.

> A ideia é parecida com o port mapping `(que inclusive deve ser usado também, criando a conexão 3000:3000)`, fazendo com que dentro do container fique referenciais às pastas da máquina local.

1. Dockerfile.dev
  > Diferente do que vimos anteriormente, aqui devemos criar um Dockerfile voltado ao desenvolvimento e não o comum, voltado à produção (algo que a gente sente a diferença, só não consegue explicar).
  > A sintaxe do Dockerfile.dev nesse caso ficou a seguinte:
>   
      ~~~dockerfile
        FROM node:16-alpine
        
        USER node
        
        RUN mkdir -p /home/node/app
        WORKDIR /home/node/app
        
        COPY --chown=node:node ./package.json ./
        RUN npm install
        COPY --chown=node:node ./ ./
        
        CMD ["npm", "run", "start"]
      ~~~

  > O build será: `docker build -f Dockerfile.dev .`
  > A flag -f serve para indicar o arquivo exato a ser construído.

2. Docker run de volumes
   > Nesse projeto em específico, para economizar tempo e espaço de memória, apagamos a pasta node_modules na máquina local.
   > Para rodar a imagem, precisamos da seguinte sintaxe: `docker run -p 3000:3000 -v /home/node/app/node_modules -v $(pwd):/home/node/app <IMAGEM-ID>`.
   > A primeira flag -v impede que o docker procure dentro da máquina local a pasta node_modules, enquanto que a segunda flag -v direciona os referenciais do container para o lugar certo.


**Palavras/frases do dia:** *Workflow, development, testing, deployment, branches, github, travis CI, AWS, docker volume*

## Dia 17 de dezembro de 2021 - Módulo 7
### Continuous Integration and Deployment

> Nesse Módulo vemos como fazer uma integração do projeto em uma máquina local com o GitHub. Além disso, vemos como conectar o GitHub com o provedor de CI, nesse caso, é o Travis CI.
> Sabe-se que a equipe de DevOps da Wine não utiliza o Travis CI em seu processo de deploy, logo o que será descrito nesse módulo é apenas para fins de aprendizado.

> O procedimento básico de testagem e deployment utilizando o Travis CI é o seguinte:
 1. Indicar ao Travis CI qual cópia do docker running nós precisamos;
 2. Construir a Imagem utilizando o Dockerfile.dev;
 3. Rodar e testar com o Travis;
 4. Realizar o deploy para a AWS.

> Para realizar o procedimento acima, precisamos indicar como o Travis CI irá realizar a leitura do projeto correto e como irá rodar os testes, entre outras configurações. Para isso, criamos dentro da pasta do projeto em questão um arquivo `.travis.yml`.
> Esse arquivo yml terá a seguinte sintaxe:
  ~~~yml
    sudo: required
    services:
      - docker

    before_install:
      - docker build -t <ID Docker>/<GitHub Repo> -f Dockerfile.dev .
  
    script:
        - docker run -e CI=true USERNAME/docker-react npm run test -- --coverage
  ~~~

> Logo após realizar o commit e o push das alterações realizadas na máquina local para o GitHub, rodamos os testes no Travis CI.
> Visto que os testes foram bem sucedidos, podemos ir para a AWS.

### AWS Elastic Beanstalk

> Após realizar login na AWS, podemos iniciar um novo app e, a partir dele, um novo Environment, aonde constará o projeto em questão que você deseja realizar um deploy. 
> Dentro da AWS Environment, nosso container ficará sitiado em uma Virtual Machine. 

> Após a criação do Environment, voltamos ao arquivo `.travis.yml` e acrescentamos logo abaixo do script:
  ~~~yml
    provider: elasticbeanstalk
    region: "<região-escolhida>" # Ex: us-west-2
    app: "<nome-do-app>"
    env: "<nome-do-environment>"
    bucket_name: "<url-do-app-gerado-automaticamente>"
    bucket_path: "<nome-do-app>" # Por default, usa-se o nome do app
    on:
      branch: master # branch do seu repo github que será usada para deploy
  ~~~

> O próximo passo é criar uma API key. O passo a passo para criar uma nova chave é vista no vídeo 101 do curso.

> Nos vídeos seguintes do curso, vemos como criar uma nova branch em um repo GitHub, usando `git checkout -b feature`. Além disso, aprendemos como é o processo de pull request dentro do dashboard do github e como o pull request conecta a branch feature com a master, permitindo então que o Travis CI faça os testes e envie logo mais para a AWS Elastic Beanstalk.

> Os três últimos arquivos do Módulo 7 são um compilado dos passos necessários para toda conexão entre Máquina Local --> GitHub --> Travis CI --> AWS.

**Palavras/Frases do dia**: *AWS Elastic Beanstalk, Travis CI, GitHub, Branches, .travis.yml, running test, deploy*. 

## Dia 21 de dezembro de 2021 - Módulo 9
### Section 9 - Dockerizing Multiple Services

> A seção 8 do curso foi pulada pois o foco não era em Docker, e sim na construção de um aplicativo JavaScript.
> Para o início do módulo 9, foi realizado o download do arquivo `checkpoint.zip`.

#### Construíndo docker-compose com postgres
> No arquivo checkpoint.zip contém três pastas: worker, server e client. Em cada uma delas, construímos o arquivo `Dockerfile.dev` padrão, já mostrado acima.
> Na pasta root do projeto (aqui chamado de complex), criamos o arquivo `docker-compose.yml` com novos atributos:
  ~~~yml
    version: '3'
    services:
      postgres:
        image: 'postgres:latest'
        environment:
          - POSTGRES_PASSWORD=postgres_password
      redis:
        image: 'redis:latest'
      nginx:
        restart: always
        depends_on:
          - api
          - client
        build:
          dockerfile: Dockerfile.dev
          context: ./nginx
        ports:
          - '3050:80'
      api:
        build:
          dockerfile: Dockerfile.dev
          context: ./server
        volumes:
          - /home/node/app/node_modules
          - ./server:/home/node/app
        environment:
          - REDIS_HOST=redis
          - REDIS_PORT=6379
          - PGUSER=postgres
          - PGHOST=postgres
          - PGDATABASE=postgres
          - PGPORT=5432  
      client: 
        stdin_open: true
        build: 
          dockerfile: Dockerfile.dev
          context: ./client
        volumes:
          - /home/node/app/node_modules
          - ./client:/home/node/app  
      worker:
        build: 
          dockerfile: Dockerfile.dev
          context: ./worker
        volumes:
          -  /home/node/app/node_modules
          - ./worker:/home/node/app
        environment:
          - REDIS_HOST=redis
          - REDIS_PORT=6379
  ~~~
> Atenção especial para o atributo context: é ele quem sinaliza qual Dockerfile.dev será utilizado na build do container.]
> Atenção também para o environment, ele determina certas permissões que seu container precisa ter para que funcione bem.
> Outra coisa que devemos olhar é a presença do serviço **nginx** para multiroteamento. Vemos o atributo restart: always nele pois, caso ocorra algum erro, o container reinicie automaticamente para que não ocorra o crash dos outros serviços relacionados. 

### Nginx
> Criamos na pasta do projeto uma nova pasta direcionada para o multiroteamento. Nela, foi criado o arquivo `default.conf` na intenção de cuidar de como os serviços serão conectados dentro do container. Todas as portas de entrada e saída de dados estão relacionadas lá.

## Dia 22 a 24 de dezembro de 2021 - Módulo 10
### A Continuous Integration Workflow for Multiple Images

> Aqui aprendemos como fazer a integração com o Travis CI e o Elastic Beanstalk da AWS.
> Nada de muito novo, apenas algumas configuraçõs dentro do Nginx para manter o roteamento de todas as portas locais configuradas para acessar as portas dentro do container.

# Dia 23 de dezembro de 2021 - Módulo 12 - Kubernetes
### Grande dia, família! Onwards to Kubernetes!

> Começamos a falar sobre Kubernetes da mesma forma que começamos com o Docker: O que é e por que usar?

1. **O que é Kubernetes?**
   > Pegue como exemplo nosso último projeto de calcular index da sequência de Fibonacci. Quando realizamos deploy para a AWS Elastic Beanstalk, subimos quatro containers (nginx, server, client e worker). Para somente uma máquina, tudo vai funcionar muito bem, mas e para várias? Qual é a escalabilidade do meu aplicativo?
   > Então, para resolver essa situação, utilizamos Kubernetes com seu conceito de cluster e nós, facilitando que vários usuários rodem diferentes containers de acordo com sua necessidade.

2. **Por que usar Kubernetes?**
   > Utilizando Kubernetes, você economiza recursos, controla o tráfego de acessos a suas imagens e containers e permite que diferentes imagens e containers rodem ao mesmo tempo.

#### Kubernetes em Desenvolvimento e Produção

1. Development
  > No desenvolvimento de aplicações com Kubernetes, dentro da sua máquina local, usaremos uma aplicação chamada **minikube**. O minikube serve para criarmos o cluster.
2. Production
  > Na produção do cluster de forma a ser acessada por outras máquinas, precisamos de gerenciadores como a Amazon Elastic Container Service for Kubernetes, ou carinhosamente chamado EKS.

> No desenvolvimento local, e apenas aqui, usaremos o minikube. Depois que o cluster já está ingressado dentro de uma máquina virtual, iremos gerenciar seus containers com o **kubectl**.

> Logo, a sequência de desenvolvimento e produção se dá por: `1) minikube criando o cluster na máquina virtual --> 2) Enviar para uma máquina virtual --> 3) gerenciar o cluster com kubectl` 

#### Multi-client Kubernetes

> No desenvolvimento de uma aplicação que visa atender vários clientes ao mesmo tempo, devemos nos atentar a três requisitos básicos:

1. Docker Hub
  > Tenha certeza que sua imagem está hospedada no docker hub <https://hub.docker.com>
  > Nesse teste, usaremos a imagem do ministrante do curso, `stephengrider/multi-client`

2. Fazer um arquivo de configuração para criar o container
  > Criaremos um novo projeto chamado `simplek8s` e, nele, o arquivo `client-pod.yaml`.

3. Fazer um arquivo de configuração para configurar a rede
  > Dentro do projeto, criaremos o arquivo `client-node-port.yaml`.

### Rodando Containers em Pods

> No mundo do kubernetes, não iremos realizar deploy em um único container, seja ele criado por docker-compose ou não. Para isso, usamos o que chamamos de Pods.

> Os Pods são a mínima parte permitida a se realizar deploys dentro do kubernetes, e todos os containers de relações intrínsecas a um container principal.

> Os Pods ficam "salvos" dentro da máquina virtual que é criada em nossa máquina local, quando rodamos o minikube. 

> Para configurarmos o Pod, usamos o arquivo `client-pod.yaml`. Nesse arquivo determinamos o nome do Pod, o tipo de componente de trabalho que ele irá atuar, os containers que ficarão contidos nele e a porta de acesso do mundo externo à ele.

> O serviço de acesso ao Pod é muito importante e é configurado pelo arquivo `client-node-port.yaml`

#### Service Config File

> Como foi dito acima, para que o mundo externo tenha contato com o Pod dentro do Kubernetes node, contido na máquina virtual criada pelo minikube dentro de nossa máquina local, é necessário que se configure o tipo de serviço que será utilizado para tal.

> Os services são responsáveis por criar as conexões do cluster com o mundo externo e existem quatro tipos deles: `ClusterIP, NodePort, LoadBalancer e Ingress`. Nesse teste usaremos o NodePort, único serviço com funções de desenvolvimento.

### Coisas importantes sobre o Kubernetes

1. Kubernetes é um sistema para realizar deploys de aplicativos em containers;
2. Nós (Nodes) são máquinas individuais (locais ou VM) que vão rodar os containers;
3. Masters são máquinas (locais ou VM) com um set de programações de gerenciamento de Nodes;
4. Kubernetes não cria suas imagens, ele pega de algum outro lugar (Ex: Docker Hub);
5. O master decide aonde rodar cada container. Cada nodecan roda um conjunto diferente de containers;
6. Para realizar deploy de algo, precisamos carregar nossos comandos para o master utilizando arquivos de configuração (Ex: client-pod.yaml);
7. O master trabalha constantemente monitorando os nodes para manter os estados de aceitação.
   
> Outra coisa importante a ser decidida é o método de entrega de ordens para o master: imperativa ou declarativa.
> Segundo o curso, em qualquer documentação será ensinada a forma imperativa, porém engenheiros que trabalham com kubernetes preferem, em grande maioria, trabalhar com ordens declarativas.