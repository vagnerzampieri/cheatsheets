# Docker

Não é necessário passar todo o ID de um CONTAINER para fazer alguma ação, passando as 3 primeiras letras o docker já entende 

```
# 026a3b92a7cd
docker start 026
```

Lista apenas os CONTAINERs ativos, o `-a` vai listar todos os CONTAINERs

```
docker ps
```

O `-a` é de atach, combinado com start e o `-i`, é possível entrar direto no modo interativo de um CONTAINER

```
docker start -a -i CONTAINER
```

Com o prune é possível remover algo, combinado com `container` ele remove todos os CONTAINERs inativos, com `docke rm 026` você irá remover um em específico

```
docker container prune
```

Layered Filesystem, uma imagem pode ser composta por várias imagens, então se você tem alguma imagem que já contêm uma parte
de uma imagem que você está baixando, o docker reutilizada esse pedaço baixado

O `-d` significa detach(desanexar) que faz com que o docker rode em background, uma imagem com `/` significa que não é uma imagem oficial e que você precisa 
especificar o usuário

```
docker run -d dockersamples/static-site
```

O `-t` faz com que o docker não espere os 10 segundos(default) antes de dar stop no CONTAINER, faz isso imediatamente

```
docker stop -t 0 026
```

O `-P` vai mapear a porta criada no docker, com as suas portas locais

```
docker run -d -P dockersamples/static-site
```

Lista as portas mapeadas de um CONTAINER para a sua máquina

```
docker port 026
```

A tag `-name` possibilita dar um nome ao CONTAINER como quisermos 

```
docker run -d -P -name meu-site dockersamples/static-site
```

A tag `-p` vai mapear a porta que você quer que seja utilizada da sua máquina, para a porta do docker

```
docker run -d -p 12345:80 -name meu-site dockersamples/static-site
```

A tag `-e` vai possibilitar passar variáveis para dentro do docker

```
docker run -d -p 12345:80 -name meu-site -e AUTHOR="John Doe" dockersamples/static-site
```

A tag `-q` vai listar apenas os ids, no caso do comando abaixo vai ser dado `stop` em todos os ids que estão rodando

```
docker stop -t 0 $(docker ps -q)
```

A tag `-v` define o volume, o primeiro parâmetro é um local definido na sua máquina para compartilhar a informação dentro do Docker,
o segundo vai ser dentro do CONTAINER, tudo que for escrito em ambos os lados será compartilhada por um 'túnel'

```
docker run -v "~/Projects/test:/var/www" ubuntu
```

A tag `-w` vai iniciar o CONTAINER na pasta definida, para que caso necessite rodar algum comando nela, não dê erro. Nesse caso estamos usando
uma imagem de `node` e rodando o comando `npm start`

```
docker run -d -p 8080:3000 -v "~/Projects/test:/var/www" ubuntu -w "/var/www" node npm start
```

## DOCKERFILE

O padrão de nome a ser criado é chamando de `Dockerfile`, caso você tenha mais de um arquivo na mesma pasta, pode chamar de `node.dockerfile`, ou `ruby.dockerfile`

```
FROM node:latest
MAINTAINER Vagner Zampieri
COPY . /var/www
WORKDIR /var/www
RUN npm install
ENTRYPOINT npm start # comando que será executado no final no processo
EXPOSE 3000
```

Caso o nome do arquivo não seja `Dockerfile`, é necessário passar a flag `-f` com o nome do arquivo. A tag `-t` com build indica o nome da imagem que está sendo criada

```
docker build -t zampi/node .
```
## REDE

Vai ser criada uma rede chamada `minha-rede` baseada no driver `bridge` que suporta 99% dos casos. Uma aplicação distribuída necessita ficar na mesma rede

```
docker network create --driver bridge minha-rede
```

Lista as redes na máquina

```
docker network ls
```

Vai adicionar a rede criada ao meu container

```
docker run -it -name container-ubuntu --network minha-rede ubuntu
```

## DOCKER COMPOSE

É usado um Load Balancer (Nginx) para distribuir entre as aplicações quem será acessada, ele também serve para controlar os assets. O Docker Compose vem
para ajudar a orquestrar isso

No exemplo abaixo é criado 5 CONTAINERS e a rede, o Load Balance (Nginx), Banco (Mongo) e 3 para aplicação (Node)

```
version: '3'
services:
    nginx:
        build:
            dockerfile: ./docker/nginx.dockerfile
            context: .
        image: zampi/nginx
        container_name: nginx
        ports:
            - "80:80"
        networks: 
            - production-network
        depends_on: 
            - "node1"
            - "node2"
            - "node3"

    mongodb:
        image: mongo
        networks: 
            - production-network

    node1:
        build:
            dockerfile: ./docker/alura-books.dockerfile
            context: . # Raíz do projeto'
        image: zampi/alura-books
        container_name: alura-books-1
        ports:
            - "3000"
        networks: 
            - production-network
        depends_on:
            - "mongodb"

    node2:
        build:
            dockerfile: ./docker/alura-books.dockerfile
            context: .
        image: zampi/alura-books
        container_name: alura-books-2
        ports:
            - "3000"
        networks: 
            - production-network
        depends_on:
            - "mongodb"

    node3:
        build:
            dockerfile: ./docker/alura-books.dockerfile
            context: .
        image: zampi/alura-books
        container_name: alura-books-3
        ports:
            - "3000"
        networks: 
            - production-network
        depends_on:
            - "mongodb"

networks: 
    production-network:
        driver: bridge
```

Criar as imagens

```
docker-compose build
```

Start CONTAINERs

```
docker-compose up
```

Lista os CONTAINERs que estão rodando

```
docker-compose ps
```

Para e remove os CONTAINERs

```
docker-compose down
```

É possível mexer em todos os CONTAINERs que foram subidos com o docker-compose. Você pode acessar através do nome do CONTAINER (alura-books-2) ou do serviço
(node2), dentro do docker-compose.yml foi utilizado o nome do serviço

```
docker exec -it alura-books-1 ping alura-books-2
```
