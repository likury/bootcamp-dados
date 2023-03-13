# Instação AWS Command Line Interface

No terminal do Ubuntu execute os seguintes comandos:

- ```curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"```
- ```unzip awscliv2.zip```

Caso não possua o unzip basta instalá-lo por meio do seguinte comando:

-```sudo apt install unzip``` 

Continuando a instalação:

- ```sudo ./aws/install```

Agora, a AWS CLI já está instalada. Vamos configurar as credenciais:

- Na AWS clique no seu nome na página inicial e selecione "Credenciais de Seguraça"
- Gere uma chave de acesso e copie os valores em outro arquivo pois não será possível vêlo-os novamente.
- No terminal execute: ```aws configure```
- Insira as chaves públicas e secretas.
- Todas as outras etapas basta pressionar enter. 
