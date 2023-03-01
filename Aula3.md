## Vamos utilizar o Dockerfile para customizar nossa imagem

### Mapeando comandos do terminal ao Dockerfile

A primeira linha de qualquer Dockerfile é a indicação de que imagem será usada como base.

Nesse caso vamos utilizar uma imagem contendo o Node.js. Isso é equivalente a instalar o node localmente.

```FROM node:13-alpine```

A segunda etapa é definir as nossas variáveis de ambientes assim como foi feito no ```docker run```.


Obs.: Não é recomendo que as variáveis de ambiente sejam definidas no Dockerfile. O motivo é que caso variáveis sejam adicionadas ou alteradas teremos que reconstruir a imagem alterada para que essas mudanças sejam aplicadas. 

O adequado é definirmos que as variáveis de ambiente sejam definidas no arquivo .yml do  ```docker compose```. Assim as variáveis podem ser alteradas em tempo de execução.

```
ENV MONGO_DB_USERNAME=admin \
    MONGO_DB_PWD=password
```

No terminal a declaração acima é equivalente aos seguintes comandos:

```
set MONGO_DB_USERNAME=admin
set MONGO_DB_PWD=password
```

Agora, vamos executar um comando do terminal a partir do Dockerfile para criar uma pasta dentro da imagem:

```
RUN mkdir -p /home/app
```

Agora copiamos o conteúdo da nossa pasta local contendo nossa aplicação para a pasta que acabou de ser criada com a linha acima:

```
COPY . /home/app
```

Então, é necessário executar o comando que iniciar com o node:

```
CMD ["node", "/home/app/server.js"]
```

Dessa forma os layers da nossa imgem são ```alpine:3.10```, ```node:13-alpine``` e ```app:1.0```.

Agora a partir do Dockerfile podemos construir a imagem customizada a partir do comando ```docker build```. Como parâmetros desse comando vamos passar o nome e tag da imagem. Também vamos informar que o Dockerfile se encontra no diretório atual usando o ponto.

```docker build -t myapp:1.0 .```

O processo acima é simulação do que a ferramenta de CI Jenkis Server faz.


### Salvando a imagem um um repositório de imagens Docker

Um serviço de repositório pode ser utilizado para guardar a imagem criada. Um desses serviços é o AWS Elasctic Container Registry. Um repositório de Docker no ECR pode conter diversas tags, isto é, versões de uma mesma imagem. 

O nome de uma imagem no repositório será ```dominioRegistro/nomeImagem:tag```.

É possível fazer alterações no app e subir essas alterações para o repositório da mesma forma que no git.   

### Fazendo o deploy da aplicação containerizada com docker compose

vamos adicionar o seguinte no .yml:
```
my-app:
    image: imageURI
    ports: 
        - 3000:3000
```

Também é necessário alterar a url do banco de dados de localhost para o nome do container do MongoDB. Nesse caso não é necessário utilizar o número da porta. Isso tornar o deploy de múltiplos containers muito mais fácil.


