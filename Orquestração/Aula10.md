# Volumes

## StorageClass

Vamos primeiro precisar criar um StorageClass para definir o volume do EBS.

Então precisamos fazer um claim desse volume que acabamos de criar na AWS. 

Depois fazemos o deployment de um container do MySql. 

Para acessar o banco pode fazer:

```kubectl run -it --rm --image=mysql:5.6 --restart=Never mysql-client -- mysql -h mysql -p dbpassword11```
-------------------------------------------------------------------------------------
Atividade: Transformar o projeto do módulo passado para Kubernetes. Entrega até 31/03.

Email: lucas.sepeda@ada.tech
Assunto: [KUBERNETES - FINAL]