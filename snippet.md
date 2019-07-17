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
