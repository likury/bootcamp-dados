## Volumes Docker

Como discutidos nas seções anteriores, toda vez que um container é reiniciado os dados armazenados são perdidos. 

Para evitar que isso aconteça, por exemplo quando a instância do banco MySQL for reiniciada, é necessário acoplar um volume ao container.

Podemos definir esse volume no docker compose. O primeiro passo é definir o volume na mesma identação que ```services``` no arquivo YAML.

```
volumes:
    mysql-data:
        driver: local 
```

Depois, é necessário declarar o volume dentro do serviço que vai utilizá-lo. São necessários o nome do volume e o caminho dentro do container que o driver será criado. No caso do MySQL, esse caminho deve ser o que o MySQL espera encontrar os dados de seus DBs. 

```
volumes:
    - mysql-db:/var/lib/mysql
```

Os volumes administrados pelo Docker se encontram em ```C:\ProgramData\docker\volumes``` no Windows e ```var/lib/docker/volumes```