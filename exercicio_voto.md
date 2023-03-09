## Criação de sistema de voto em cluster

Neste exercicio vamos utilizar exemplos do repositório do docker para criar uma aplicação de votação. 

Nossa aplicação será composta dos seguintes serviços:

- vote: aplicação frontend em python que permite ao usuário registrar um voto
- redis: serviço de notificação que recebe o dado do frontend e repassa para uma aplicação .NET que faz o processamento
- worker: aplicação em .NET que recebe o dado e repassa a um banco de dados
- db: serviço de banco de dados em postgresql que armazena os dados processados pelo worker
- result: aplicação frontend que lê os dados


As imagens são as seguintes:

- redis: redis:alpine
- db: postgres:15-alpine
- vote: dockersamples/examplevotingapp_vote
- result: dockersamples/examplevotingapp_result
- worker: dockersamples/examplevotingapp_worker

As configurações necessárias são as seguintes:

- Liberar a porta TCP e UDP 7946
- Liberar a porta UDP 4789
- Liberar a porta TCP 6379

- O db exige as seguintes variáveis de ambiente:

POSTGRES_USER: "postgres"

POSTGRES_PASSWORD: "postgres"

- Também podemos adiconar um volume que salve o caminho dos dados armazenados. O caminho que o postgresql armazena os dados é: /var/lib/postgresql/data

- O serviço vote roda na porta 80 dentro do container. Faça um mapeamente para que o serviço seja acessado pela porta 80. 

- O serviço result roda na porta 80 dentro do container. Faça um mapeamento para que o serviço seja acessado pela porta 8080.

Uma vez criada a stack podemos executá-la:

```docker stack deploy -c docker-stack.yml vote```




Entrega: enviar .yml pro email lucas.sepeda@ada.tech com assunto [SWARM-STACK]