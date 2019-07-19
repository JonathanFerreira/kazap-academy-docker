##############################################################################
##############################################################################
##############################################################################
########## PARTE 1 - CRIANDO NOSSA PRIMEIRA IMAGEM E CONTAINER ###############

## Criar um dockerfile com o conteudo

```
FROM ubuntu
MAINTAINER Jonathan Ferreira
```

## Criando uma imagem a partir do dockerfile

```
docker build -t jvferreira/nodejs .
```

## Criando um container a partir da imagem jvferreira/nodejs
### NOTAS
* --name => Dando um nome ao container, caso contrario ele criará um randômico
* -it => É um option que permite executar o prompt bash no container após a criação
* jvferreira/nodejs => Minha imagem criada anteriormente

```
docker run -it --name ambiente-nodejs jvferreira/nodejs
```

## Instalar o nodejs

### Primeiro precisamos atualizar os repositórios do container com o ubuntu

```
apt-get update
```

### Depois vamos instalar o nodejs

```
apt-get install nodejs
```

### Testando a instalação

```
node -v
```

## Vamos commitar essa nova imagem com o node instalado

### Isso irá salvar as alterações que eu fiz na imagens, no caso a instalação
### do node. Então a partir de agora qualquer container que eu criar a partir
### dessa imagem já terá o node instalado!

```
docker commit ambiente-nodejs jvferreira/nodejs:1.0.0
```

### Então agora nós podemos remover aquele container antigo que foi criado
### manualmente e criar um novo a partir da nova imagem

```
docker run -it --name ambiente-node jvferreira/nodejs:1.0.0
```

### Pronto! Mais tarde falaremos sobre versionamento de imagens no Dockerhub!

##############################################################################
##############################################################################
##############################################################################
####################### PARTE 2 - TUDO NO DOCKERFILE #########################

## Agora vamos passar tudo isso para o dockerfile e facilitar nossa vida

### Complementar o dockerfile com o conteúdo

```
RUN apt-get update

RUN apt-get install nodejs -y

RUN node -v
```

## Criando a nova imagem a partir do dockerfile completo

```
docker build -t jvferreira/nodejs-completo .
```

## Criando um novo container a partir da imagem completa

### NOTAS
### -v => Criação de volumes
### Os volumes são o mecanismos para persistir dados gerados e usados ​​pelos contêineres

```
docker run -it --name ambiente-nodejs-completo -v $(pwd)/app:/usr/app jvferreira/nodejs-completo
```

##############################################################################
##############################################################################
##############################################################################
############## PARTE 3 - Vamos falar de versionamento - Dockerhub ############


## O dockerhub de maneira simples é um repositório de imagens.

## Vamos versionar nossa imagem

1 - Temos que criar uma conta no dockerhub
2 - docker login
3 - Inserir o usuario e senha

## Commitando a imagem como fizemos anteriormente

```
docker commit ambiente-nodejs-completo jvferreira/nodejs-completo:1.0.0
```

## Subindo a imagem

```
docker push jvferreira/nodejs-completo:1.0.0
```

Pronto! Versionamos nossa imagem no Dockerhub!

##############################################################################
##############################################################################
##############################################################################
#################### PARTE 4 - Explicando Comandos Docker ####################

## Docker pull
Baixa uma imagem do DockerHub
```
docker pull NOME_DA_IMAGE
```

## Docker inspect

O comando docker inspect retorna um `json` contendo todas as informações referentes as configurações do container.

```
docker inspect NOMO_DO_CONTAINER
```

## Docker logs
Apresenta as informações do que está sendo enviando para a console do container.

```
docker logs NOME_DO_CONTAINER
```

## CMD x ENTRYPOINT
Dispara uma função à partir do momento em que o container é estanciado

```
ENTRYPOINT ["/bin/echo", "Hello"]
CMD ["world"]
```

## Acessando o container que fechou com EXIT
Entra no bash e da um EXIT
Depois
```
docker start NOME_DO_CONTAINER
```

```
docker ps
```

```
docker attach NOME_DO_CONTAINER
```
## User
Este comando é responsável por alterar o usuário e permissões de um container.

```
docker run -it --name NOME_DO_CONTAINER -u $(id -u):$(id -g) NOME_DA_IMAGE
```

## Attach
O comando `docker attach` acessa o `bash` que está estanciado no container

```
docker attach NOME_DO_CONTAINER
```

## Exec
O comando `docker exec` acessa o `bash` do container, porém, diferente do `docker attach` ele estancia um novo `bash`.

```
docker exec -it NOME_DO_CONTAINER bash
```

## Memory
Comando docker que limita a memória do container

```
--memory="512M"
```

## Restart
realiza uma ação quando o docker cai
```
--restart no
--restart on-failure
--restart always
--restart unless-stopped
```
Mostrar na prática o comando restart
Criar um container iniciando com o bash aberto e utilizando o parametro (--restart always)
  sair do container com EXIT
  DOCKER PS pra mostrar que o container foi startado novamente

## Volume
Mostrar como criar um volume no comando RUN
  E criar um container com volume excluir e mostrar que os arquivos criados no
    HOST ainda continua lá e os do container se perderam

## Linkagem de Containers
Mostrar o commando
  run --link NOME_DO_CONTAINER:ALIAS_DO_LINK
Pingar no alias do link para mostrar o funcionamento

## Docker attach x Docker exec
Mostrar na pratica obs (Iniciar a criação de um novo projeto em rails e apertar
  CTRL+P+Q e entrar no container com o camando ATTACH e EXEC)

## Network
Tipos de network
Diferença entre elas
Como criar uma network
  docker network create -d NOME_DO_DRIVE NOME_DA_NETWORK
Listar network's
  docker network ls
Remover network
  docker rm NOME_DA_NETWORK
Adicionar um container a uma network criada
  docker network connect NOME_DA_NETWORK NOME_DO_CONTAINER

**Drivers**

 1. **Bridge** - O driver `bridge` cria uma rede privada interna para o host para que os containers dessa rede possam se comunicar. O acesso externo é concedido pela exposição de portas a containers. O Docker protege a rede gerenciando regras que bloqueiam a conectividade entre diferentes redes Docker.
 2. **Host** - A opção `host` é usada para fazer com que os programas dentro do contêiner do Docker pareçam estar sendo executados no próprio host, da perspectiva da rede. Ele permite ao contêiner maior acesso à rede do que normalmente consegue.

 ##############################################################################
 ##############################################################################
 ##############################################################################
 ############################# PARTE 5 - Exercícios #############################

  1. Baixar uma imagem do DockerHub e gerar um container a partir da imagem
  2. Refatorar o Dockerfile de acordo com as boas práticas explicadas anteriormente.
  requisitos: Expor uma porta e alterar o diretório raiz do container.  
