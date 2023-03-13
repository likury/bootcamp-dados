# Vamos rodar um container do Nginx

## Puxando e rodando a imagem
```docker run -d -p 8080:80 --name webserver nginx```

## Listando os containers
```docker ps```

## Listando imagens
```docker images```

## Acoplar ao container
```docker container exec -it webserver bash```

## Para o container 
```docker stop webserver```

## Remover a imagem
```docker rmi nginx```

