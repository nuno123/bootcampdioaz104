# bootcampdioaz104
Bootcamp da Dio AZ-104 Microsoft Azure
Gerenciando VMs Azure 

Este documento detalha o processo de gerenciamento de VMs no Azure, incluindo requisitos, testes e observações. 

Importante:
Para criar uma VM no Azure, é necessário ter uma assinatura ativa. 
![image](https://github.com/user-attachments/assets/75187e91-a82c-4b96-aa77-490754fc1542)


Todos os recursos no Azure devem estar contidos em uma assinatura e, posteriormente, em um grupo de recursos. 
![image](https://github.com/user-attachments/assets/f2d4a972-32f7-4008-8dab-546124eeecdb)


É crucial planejar a arquitetura de redes e sub-redes para evitar conflitos com sua rede on-premise e outras nuvens. 

Ao criar uma VM, você precisará informar a assinatura, o grupo de recursos, o tipo de disco e o tipo de sistema operacional. Se a rede e a sub-rede não existirem, o Azure sugerirá a criação. Não é possível criar uma VM sem uma VNet e sub-rede associadas. 

Ao criar a VM, você pode optar por excluir a NIC (Network Interface Card) ao deletar a VM; se essa opção não for marcada, a NIC não será deletada. 

![image](https://github.com/user-attachments/assets/9ddca380-4dea-4370-9fb2-c5368e47c744)


Criação e Conexão da VM
A VM utilizada para os testes é Linux e está no nível gratuito (free tier). 

Ao escolher Linux, você pode criar um usuário local ou usar uma chave SSH própria. No exemplo, foi criado apenas um usuário e senha, e a VM foi configurada com um IP público para login. 

A demonstração de login na VM Linux é feita utilizando o endereço IP público. 
![image](https://github.com/user-attachments/assets/a355902b-6b71-48a2-b69b-14485f8c9fda)


Apesar de ter um IP público, o acesso à VM foi limitado ao IP da residência do usuário para evitar tentativas de ataques durante os testes. 

Essa configuração de segurança pode ser realizada nas "Network settings" da VM. 

![image](https://github.com/user-attachments/assets/b42ce09b-1810-4d8a-bdba-97da2c365c22)


A conexão ao servidor Linux é realizada via SSH, solicitando a senha do usuário. A VM em questão é um Ubuntu 22.04.5 LTS. 

![image](https://github.com/user-attachments/assets/86f483e5-83b5-44fc-a068-7ee95d0aee0b)

![image](https://github.com/user-attachments/assets/bbb5b716-7fbf-4ef8-8936-6b443d2bf442)



Adicionar e Montar um Disco
Após a conexão, o próximo passo é adicionar um disco à VM Linux, anexá-lo e, em seguida, montá-lo. 

![image](https://github.com/user-attachments/assets/a7ac4f32-465e-4ab1-9995-2b9faf55b998)

![image](https://github.com/user-attachments/assets/451ee0f8-49fa-4b2a-b87b-ad44a63e3fdc)



Para identificar os discos, utiliza-se o comando fdisk -l. No exemplo, o disco 

/dev/sdc de 4 GiB é um dos discos listados. 

Após identificar o disco (

/dev/sda no exemplo de formatação), ele é formatado com o sistema de arquivos ext4. 

Bash

sudo mkfs -t ext4 /dev/sda 

Em seguida, o disco é montado no diretório

![image](https://github.com/user-attachments/assets/ec620205-9d2d-4c99-acfe-e7290edbb731)


/mnt/dados. 

Bash

sudo mount /dev/sda /mnt/dados 
A verificação da montagem é feita com o comando 

df -h, que mostra /dev/sda montado em /mnt/dados. 

![image](https://github.com/user-attachments/assets/d518ae91-20e1-45bd-8c5f-fefe415a5984)


Remoção do Disco
O processo de adição de disco pode ser feito "a quente", ou seja, com a VM ligada. 

![image](https://github.com/user-attachments/assets/5eb224ba-6254-484c-99d0-1a9b2b8f9a9c)

![image](https://github.com/user-attachments/assets/521015e9-7f82-4e31-bd96-c6a0b39cb21d)


É possível desanexar um disco da VM. 

Após a remoção do disco no portal do Azure, realiza-se a desmontagem no Linux e a listagem dos discos para confirmar a remoção. 

![image](https://github.com/user-attachments/assets/25c55f54-92db-4883-bde1-fdb567003853)

![image](https://github.com/user-attachments/assets/2da424e0-f61a-462d-9847-f152fbcaba1f)



Monitoramento
Na seção de monitoramento, é possível verificar o consumo de CPU, memória e disco da VM. 

![image](https://github.com/user-attachments/assets/10c23870-17c5-4f04-b41f-c34eb8e796c8)


Exclusão de Recursos
Para evitar cobranças desnecessárias, todos os recursos dentro do grupo de recursos podem ser destruídos após os testes. 


Observação: Se você deletar a VM, mas deixar os discos, eles ainda serão cobrados pela Microsoft, mesmo que não estejam anexados a VMs. 

Mesmo após desanexar um disco, ele ainda existirá no grupo de recursos. 

![image](https://github.com/user-attachments/assets/0447fda2-330f-4e5f-ba38-ac04d676a0a4)


Em ambientes de produção, é preciso ter muito cuidado ao deletar grupos de recursos, pois todos os recursos contidos neles serão excluídos junto com o grupo. 

![image](https://github.com/user-attachments/assets/3c56129d-3020-4241-9cea-49f7b01d6481)


Azure monitor + Alert groups

Gerenciamento de VM – Azure Monitor e Alertas
Este documento demonstra como utilizar o Azure Monitor em uma VM Linux para gerar alertas de e-mail quando a VM é excluída.

1. Selecionando a VM para Monitoramento
Primeiramente, selecione a máquina virtual para a qual você deseja configurar os alertas. Para este exemplo, criamos uma VM Linux para realizar os testes.
![image](https://github.com/user-attachments/assets/4ca0fe63-8e8b-4cdb-81ca-3114fbb5b37f)


2. Ativando o Agente do Azure Monitor
Para começar a coletar dados da VM, é necessário ativar o agente do Azure Monitor. Siga os passos abaixo:

Navegue até a sua VM no portal do Azure.

No painel esquerdo, em Monitoramento, clique em Insights.

![image](https://github.com/user-attachments/assets/f91e7f0c-6e53-4297-a0ce-95fc2d63c27f)
![image](https://github.com/user-attachments/assets/07da2c6d-f4d0-4557-a51b-ff39bae55743)
![image](https://github.com/user-attachments/assets/0e03a3a5-502f-4250-9e56-38ef517ad8eb)
![image](https://github.com/user-attachments/assets/a63f3656-dfcd-48bc-8887-62ad0885b549)


Selecione Habilitar.

Ao realizar esse processo, o Azure criará um Log Analytics Workspace. Este workspace armazenará as métricas e logs, permitindo que você faça consultas usando KQL (Kusto Query Language) e monitore o recurso de forma eficaz.

![image](https://github.com/user-attachments/assets/65211766-6286-4e3a-bf98-ccd6ea8bd0ed)


3. Criando uma Regra de Alerta para Exclusão da VM
Agora, vamos configurar uma regra de alerta para ser notificado por e-mail quando a VM for excluída.

No portal do Azure, procure por Monitor e clique em Alertas.

Clique em Criar e depois em Regra de alerta.

![image](https://github.com/user-attachments/assets/edbca1c2-8598-413c-9e1b-19b191baba0a)
![image](https://github.com/user-attachments/assets/a440ce50-6937-4020-9bcc-617a7529c8db)
![image](https://github.com/user-attachments/assets/61260465-1ef0-4a5a-9f18-292614f77135)
![image](https://github.com/user-attachments/assets/e43ff5b2-35ee-4bfc-b92a-f3b395ec4b22)
![image](https://github.com/user-attachments/assets/5018c87f-5505-4c2d-906f-c4aa10be2727)
![image](https://github.com/user-attachments/assets/82ae4653-3a10-456a-8d88-7e5e93018fd6)
![image](https://github.com/user-attachments/assets/95775945-7e53-4cf0-8b31-3a9dd02a9d3f)
![image](https://github.com/user-attachments/assets/9054cf86-4d33-43cd-97e5-809a44ff3a4d)
![image](https://github.com/user-attachments/assets/d1edb455-c4ab-40c8-9f90-46c69480f407)




Na seção Escopo, selecione sua assinatura e o recurso (sua VM). Clique em Próximo: Condição.

Na seção Condição, clique em Adicionar condição.

Em Tipo de sinal, selecione Logs de Atividade.

Na caixa de pesquisa, digite Delete Virtual Machine e selecione o evento correspondente. Isso configurará o alerta para disparar quando houver uma operação de exclusão de VM. Clique em Concluído.

Clique em Próximo: Ações.

4. Criando um Grupo de Ações (Action Group)
Um Action Group define quem deve ser notificado e como, quando um alerta é disparado.

Na seção Ações, clique em Criar grupo de ações.

Na guia Noções básicas:

Preencha o Nome do grupo de ações (por exemplo, NotificacaoExclusaoVM).

Preencha o Nome de exibição (será o nome exibido no e-mail de notificação).

Selecione o Grupo de recursos onde o Action Group será criado.

Na guia Notificações:

Em Tipo de notificação, selecione Email/SMS/Push/Voz.

Preencha o Nome da notificação.

Em Email, insira o endereço de e-mail para onde o alerta deve ser enviado.

Clique em Revisar + criar e depois em Criar.

5. Associando o Action Group à Regra de Alerta
Após criar o Action Group, você retornará à tela de criação da regra de alerta.

Na seção Ações, em Grupos de ações, selecione o Action Group que você acabou de criar.

Clique em Próximo: Detalhes.

Na seção Detalhes:

Preencha o Nome da regra de alerta (por exemplo, AlertaDelecaoVM).

Escolha a Severidade do alerta.

Marque a opção Habilitar regra de alerta na criação.

Clique em Revisar + criar e depois em Criar.

6. Testando o Alerta: Excluindo a VM
Agora, vamos testar se o alerta será enviado conforme o esperado.

![image](https://github.com/user-attachments/assets/4166e5b0-8c1b-474e-9324-37b90f570d7b)


Navegue até a sua VM no portal do Azure.

Clique em Excluir e confirme a exclusão da VM.
![image](https://github.com/user-attachments/assets/520fc834-5740-4934-bb1f-ddfca27a6607)


Confirmação e Verificação do Alerta
Ao cadastrar o alerta, uma confirmação será enviada para o e-mail que você configurou no Azure Monitor Action Group.

![image](https://github.com/user-attachments/assets/e7477d21-cf1c-48e8-b98e-978b330d5db7)


Como podemos ver na imagem abaixo, o alerta chegou no e-mail conforme o esperado, notificando sobre a exclusão da VM:

![image](https://github.com/user-attachments/assets/e5514657-b7e8-4b7d-9d64-e3037df9b2d0)


Visualizando Alertas no Azure Monitor
Você pode verificar todos os alertas enviados acessando a seção Monitor > Alertas no portal do Azure. Lá, você terá uma visão geral de todos os alertas disparados, seu status e detalhes.
![image](https://github.com/user-attachments/assets/2357f15e-b614-49bb-af95-8a99cfbebfc2)


