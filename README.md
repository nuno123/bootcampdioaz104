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

