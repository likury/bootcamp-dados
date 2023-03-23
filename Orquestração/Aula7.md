# Insatlação e configuração do ALB

Primeiro devemos associar um provedor de identidade ao **eksctl**. Fazemos isso usando o OpenID Connect.

```
eksctl utils associate-iam-oidc-provider \
    --region region-code \
    --cluster <cluter-name> \
    --approve
```

Agora, é necessário criar uma Role para o componente que executará o controller do ALB. Primeiro fazemos o download da política:
```
curl -o iam_policy_latest.json https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/main/docs/install/iam_policy.json
```

Então, podemos criar a política pela CLI da AWS:
```
aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy \
    --policy-document file://iam_policy_latest.json
```

## Criando o ServiceAccount:

```
eksctl create iamserviceaccount \
  --cluster=my_cluster \
  --namespace=kube-system \
  --name=aws-load-balancer-controller \ 
  --attach-policy-arn= <policy_arn> \
  --override-existing-serviceaccounts \
  --approve
```

Verifique se o ServiceAccount foi criado:

```
kubectl get sa -n kube-system
```

## Instalar o AWS Load Balancer Controller Usando o Helm:

Primeiro precisamos instalar o Helm:

- Baixe o arquivo binário: https://get.helm.sh/helm-v3.11.2-linux-amd64.tar.gz
- Extraia o arquivo **helm** e coloque na pasta */usr/local/bin/*

Para checar se o a instalação deu certo:
```
helm version
```

Obs: É normal receber Warnings nesse passo.

Agora com o helm vamos instalar o controller. Primeiro adicionamos o repositório da AWS ao helm:
``` 
helm repo add eks https://aws.github.io/eks-charts
helm update
```

Agora podemos instalar o controller:
```
helm install aws-load-balancer-controller eks/aws-load-balancer-controller \
    -n kube-system \
    --set clusterName=my-cluster \
    --set serviceAccount.create=false \
    --set serviceAccount.name=aws-load-balancer-controller \
    --set region=sa-east-1 \
    --set vpcId= \
    --set image.repository=602401143452.dkr.ecr.sa-east-1.amazonaws.com/amazon/aws-load-balancer-controller
```

Aqui você deve inserir o id da VPC do cluster.

Para verificar se a instalação ocorreu corretamente:
```
kubectl -n kube-system get deployment aws-load-balancer-controller
kubectl -n kube-system describe deployment aws-load-balancer-controller
```
