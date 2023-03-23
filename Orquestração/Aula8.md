1. Criar a política do arquivo iam_policy_volumes.json
2. Obter o ARN da Role do Cluster:

```kubectl -n kube-system describe configmap aws-auth```

3. Abrir o IAM -> Funções -> Pesquisa a role obtida -> Clicar em anexar políticas -> Anexar a política criada

4.Instalar o Driver do EBS:
```
kubectl apply -k "github.com/kubernetes-sigs/aws-ebs-csi-driver/deploy/kubernetes/overlays/stable/?ref=master"
```