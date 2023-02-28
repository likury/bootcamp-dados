## Vamos criar um site que possui um banco de dados de cadastro a partir de uma imagem do Node

1. Para salvar as informações do usuários vamos utilizar o MongoDB
2. Vamos utilizar duas imagens para isso: mongo e mongo-express (utilizado para acessar o mongo pelo browser)
3. Precisamos criar a conexão (Network) entre os dois containers criados a partir dessas duas imagens

Para listar as conexões existentes executamos os seguintes comandos:

```docker network ls```

Vamos criar uma conexão para nossos containers do mongo:

```docker network create [networkName]```

Executamos as os containers:

```
docker run -d \
-p 27017:27017 \ 
-e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password \ 
--name mongodb 
--net mongo-network \
mongo
```

Podemos olhar o que aconteceu dentro do container:

```docker logs 9a38a3ffa601518edc744364bb38b8efe808f7e4613b1de2751e8c60bb65ce8f```

Agora vamos executar o mongo-express:

```
docker run -d \ 
-p 8081:8081 \ 
-e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=password \
--net mongo-network \ 
--name mongo-express \
-e ME_CONFIG_MONGODB_SERVER=mongodb \
mongo-express
```
Finalmente, para executar o projeto
```
npm install express
npm install mongodb
cd app
node server.js
```

Percebemos que estamos criando diversos containers para uma mesma aplicação. Esse processo pode ser facilitado por meio do docker compose. 

O comando docker compose lê as intruções a partir de um arquivo yaml e executa os comandos de criação dos containers. Essa abordagem é melhor pois a estruturação e leitura do que está sendo criando são muito mais claras.

Perceba que não precisamos criar a network. O docker compose cuida disso para gente.

Podemos executar o docker compose com finalidade de executar os containeirs da seguinte forma:

```docker compose -f [nomeArquivoYML] up```


