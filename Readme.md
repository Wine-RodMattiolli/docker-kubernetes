
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
* Imagem: _Arquivo único com todas as dependências e configurações requeridas para rodar um programa._
* Contêiner: Instância da imagem, roda o programa em si

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
      $ sudo apt-get install docker-ce docker-ce-cli containerd.io

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
  *Ex: $ sudo docker run hello-world*

> utilizando a imagem do Busy Box, podemos nos aprofundar muito mais na usabilidade do Docker:
  $ sudo docker run busybox echo aposta 10 no galo
    *aposta 10 no galo*

> Nesse caso, temos o comando 'echo' passando um parâmetro para ser printado na tela. Nesse caso, isso se deve ao fato da imagem BusyBox possuir uma série de atributos que podem ser usados. 
Caso tentássemos usar o comando 'echo' no contêiner hello-world, por exemplo, o Docker retornaria um erro, pois a imagem hello-world não possui esse comando. 

> esses comandos são chamados de *running process*.


