# EKS

## O que é EKS?

EKS é o serviço de Kubernetes da AWS. Nesse serviço a AWS gerencia o Master Node, instalando todas as aplicações necessárias para que o Kubernetes funcione. 

No EKS, a AWS também cuida de backups do cluster e de *scaling*. Com essa parte administrada pela AWS, o time de DevOps consegue focar mais em fazer o deploy das aplicações. Isto é, no EKS, apenas criamos e administramos apenas os Workers Nodes.

## Como usar EKS?

Passos essenciais para seguir nossa demonstração são: criar uma conta AWS, criar uma VPC e criar uma role IAM com um Security Group. Isso significa que precisamos criar um usuário da AWS que tenha permissão para realizar ações do EKS. 

Criados esses passos, vamos criar o Control Plane. Nesse passo vamos selecionar o nome do cluster, a versão do k8s, a região, qual o poder computacional do cluster etc. 

Esses processos podem ser feitos tanto pelo console quando pela AWS CLI. 

Depois de criado o Control Plane é possível se conectar ao cluster e criar Worker Nodes. No EKS, os nodes são tratados como grupos (*Node Groups*) e não instâncias individuais do EC2. 

Perceba que isso é muito trabalho para criar apenas um cluster do EKS. Para faciliar esse processo podemos usar uma ferramenta da linha de comando chamada **eksctl**. Assim, em vez de fazer cada um dos passos acima manualmente, podemos executar apenas um comando do eksctl que faz tudo isso automaticamente. 

## Usando eksctl

### Instalando eksctl

Para instalar a versão mais recente:

```
 curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
```

Podemos checar se a instalação foi bem sucedida com: 
```eksctl version```

O output deve ser a versão da instalação. 

Também será necessário instalar o **aws-iam-autheticator**:

Os seguintes comandos no Linux instalam o aws-iam-authenticator.

Faça o download do binário:

```
curl -Lo aws-iam-authenticator https://github.com/kubernetes-sigs/aws-iam-authenticator/releases/download/v0.5.9/aws-iam-authenticator_0.5.9_linux_amd64
```

Aplique permissões de execução ao binário:

```
chmod +x ./aws-iam-authenticator
```

Copie o binário para um pasta no $PATH:



Teste a instalação:

```
aws-iam-authenticator help
```

## Criando um cluster com o eksctl

Para criar um cluster podemos executar o seguinte comando:

```
eksctl create cluster \
--name my-cluster \
--version 1.25 \
--region sa-east-1 \
--nodegroup-name my-linux-nodes \
--node-type t2.micro \
--nodes 2
```

## Fazendo o deploy das aplicações

Vamos fazer o deploy de uma aplicação de teste a partir da imagem ```paulbouwer/hello-kubernetes:1.5```. 

Exercício: 

- Criar um Deployment para uma aplicação chamada **hello-kubernetes**. 

- Vamos criar um deployment com 3 pods para *high avalability*. A porta no container é a porta 8080 

- Criar um service do tipo *LoadBalancer*.

Entrega do YML: lucas.sepeda@ada.tech - Assunto: [KUBERNETES - EKS]

## "Pausando" o cluster

```eksctl scale nodegroup --cluster my-cluster --name my-linux-nodes --nodes 0 --nodes-max 1 --nodes-min 0```

## Deletando o cluster
```eksctl delete cluster --region=sa-east-1 --name=my-cluster```