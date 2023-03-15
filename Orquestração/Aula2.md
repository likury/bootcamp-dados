## Service x Deployment

Os dois componentes são abstrações do Kubernetes.

### Deployment

Um *Deployment* é um objeto do cluster que representa um grupo de um conjunto de *Pods* rodando, um grupo lógico que tem propriedades de gerenciamente atreladas a ele. Os *Deployments* possuem a semâtica para o estado desejado de determinado grupo de *Pods*

Por exemplo, suponha que é necessário adicionar ao cluster um suporte para análise de métricas de determinados serviços. O time de DevOps cria uma imagem Docker chamada **metrics-analyzer-1.0** que recebe em sua configuração *onde* e *quais* agentes devem ser monitorados, *com que frequência* esses dados devem ser coletados, e *qual endpoint* de cada agente fornece as métricas. 

Agora, se o time de DevOps simplesmente cria um *Pod* **metrics.yml** e faz o deploy para o Kubernetes, será criado apenas um *Pod* rodando em algum *Node* que foi escalado pelo Kubernetes. Se alguma operação falhar, ou a imagem de métricas precisar ser atualizada, ou o Kubernetes precisar fazer alguma realocação de recursos o *Pod* provavelmente se tornará indisponível causando **downtime** ao coletor de métricas. 

Um *Deployment* ajuda você a criar um grupo de *Pods* **metrics-analyzer** identificando-os por *labels* para você ser capaz de gerenciar seu coletor de métricas sem ter que se preocupar com cada *Pod* individualmente. 

Essencialmente, *Deployments* no Kubernetes falam como agrupar os containers usando *labels*, o número de réplicas necessárias e como elas devem ser implementadas. 

Também, no **spec** de *Deployments* podemos definir o **spec** dos *Pods*, evitando ter vários arquivos com o mesmo propósito.


### Services:

*Services* por outro lado, agrupa os *Pods* com o único próposito de *network communication*.

O escopo de trabalho desse componente é fornercer uma série de regras quanto à infraestrutura de rede de um grupo de *Pods*. Algumas dessas regras são:

- Quais *Pods* fazem parte do grupo de serviço.
- Que tipo de rede deve ser utilizada: Nodeport, ClusterIp, Load Balancer, External Name
- Protocolos e portas. Para onde o tráfego deve ser direcionado quando houver um request para o Service
- O nome da network do serviço

*Services* permitem que outros *Pods* com componentes do cluster sem a necessidade de saber diratamente seu endereço do IP no cluster. 







