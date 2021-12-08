
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
    FS Snapshot | Startup Command
    ----------- | ---------------
    bin, dev, etc, home, proc | {Algum comando de inicialização}

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
    > $ sudo apt-get update
    > $ sudo apt-get install docker-ce docker-ce-cli containerd.io

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
    > flag -i permite a entrada de dados;
    > flag -t formata/identa a entrada de dados;
  * O comando `sh` permite trabalhar em uma shell, para que não seja necessário repetir docker exec toda vez que for preciso executar um novo comando. 

**Palavras/frases do dia:** *docker system prune, docker stop, docker kill, docker exec -it, sh. Multi-comandos, Shell*

## Módulo 3 - Building Custom Images Through Docker Server

> Sabemos que todos os containers foram construídos por alguém. Portanto, também é extremamente válido aprender como construir nossos próprios containers. Basicamente, a construção de um container segue o seguinte fluxo:

    Docker File | -> | Docker Client | -> | Docker Server | -> | Imagem 

#### Criando um Dockerfile
> O primeiro passo para a criação de uma Imagem Docker é construir seu Dockerfile. O Dockerfile é um arquivo sem extensão que possui algumas palavras reservadas. Como exemplo de criação de um Dockerfile clone do Redis-client, temos:

  Instrução p/ Server Docker | Argumento da Instrução
  :------------------------: | :--------------------:
  FROM                       | alpine
  RUN                        | apk add --update redis
  CMD                        | ["redis-server"]

> Alpine é uma imagem baseada na distribuição Linux de mesmo nome, que possui características como gestão de pacotes apk e execução sobre a RAM. 

> logo após a criação do Dockerfile, podemos executa-lo pelo terminal usando `$ docker build .`, dentro do diretório origem do arquivo. 