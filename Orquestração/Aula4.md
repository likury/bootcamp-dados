# O que é um Namespace?

Nos clusters de K8s é possível organizar os recursos em *Namespaces*. Pode-se pensar no namespace como um cluster virtual dentro de um cluster de Kubernetes.

Quando um cluster é criado, alguns *namespaces* são criados por padrão. Vamos usar ```kubectl get namespace```.

## kube-system namespace
Esse namespace não deve ser utilizado e portanto não se deve criar recursos dentro dele. 

Os componentes que estão dentro desse namespace são os processos internos do sistema, como por exemplo **kubectl**.

## kube-public
Esse contém todos os dados que estão disponíveis publicamente. Possui um **ConfigMap** que possui informações sobre o cluster que podem ser acessadas com ou se autenticação. 

## kube-node-lease
Esse namespace contém informações sobre os heartbeats dos nodes. Cada node ganha um objeto associado dentro desse namespace que é responsável por monitorar a disponibilidade do nó. 

## default 
É o namespace em que os componentes declarados pelo usuário são criados por padrão. 

## Criando novos namespaces
É possível criar novos namespaces com o comando ```kubectl create namespace my-namespace```

## Por que utilizar namespaces?
Imagine que você tem uma aplicação complexa que possui banco de dados, apps de monitoramento, serviços de ingresso etc. Rapidamente o *namespace* **default** ficará cheio de recursos e seu gerenciamento pode ficar complexo. Pode-se então criar um namespace para cada um desses grupos de recursos. 

De acordo com a documentação do Kubernetes não se deve utilizar *namespaces* se você tem uma aplicação pequena administrada por até 10 usuários. 

Outro caso de uso interessante é quando há dois times trabalhando no mesmo cluster. Suponha que um time faça um *Deployment* com o nome **my-app-deployment**. Caso outro time crie acidentalmente outro *Deployment* com o mesmo nome isso irá sobrescrever o primeiro. 

Mais um caso de uso é separar em ambientes de *Staging* e *Production*. Assim podemos aproveitar um cluster para os dois ambientes sem a necessidade de criação de um segundo cluster. 

A maioria dos recursos de um namespace não podem ser acessados por recursos em outro namespace. Uma exceção disso é o componente Service. 

Também existem recursos que não podem ser criados dentro de um namespace. Exemplos são: volumes e nodes. Para verificar todos que não podem basta executar ```kubectl api-resources --namespaced=false```

## Criando recursos dentro de um namespace
É possível utilizar o comando de apply com a seguinte flag ```kubectl apply -f app.yaml --namespace=mynamespace```.

Outra forma é declarar isso diretamente dentro do YAML. 


# Componente: Ingress

## Ingress vs. External Service

Suponha que temos uma aplicação rodando em um pod que está associado a um serviço **my-app service**. Com esse tipo de configuração podemos acessar a aplicação por meio do endereço IP do *service*. Por exemplo: http://124.89.101.2:35019. Contudo, gostaríamos que nosso produto final possa ser acessado por uma URL do tipo https://my-app.com. A forma de fazer isso é usando o componente do Kubernets *Ingress*.

Para que nosso *Ingress* funcione precisamos de um *Ingress controller*. Isso é uma série de pods que recebe, processa e redireciona todas as requisições de rede. Essencialmente, o *Ingress controller* é o entrypoint do nosso cluster. Existem diversas implementações desse controlador, por exemplo **K8s Nginx Ingress Controller**.

No nosso caso vamos utilizar o **minikube** para configurarmos o entrypoint do nosso cluster. 