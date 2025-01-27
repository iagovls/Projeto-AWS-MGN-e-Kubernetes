# Projeto migração com AWS MGN e Kubernetes

<div align="center">

[![My Skills](https://skillicons.dev/icons?i=aws,kubernetes)](https://skillicons.dev)

</div>

Este projeto é uma atividade conceitual solicitada pela equipe de estágio da Compass.UOL. O objetivo é fazer dois diagramas, um demonstrando como seria o processo de Lift-and-Shift de três servidores locais para a AWS. O outro diagrama demonstrando como seria a migração desse conjunto para uma arquitetura com Kubernetes.

# Contexto 🗎

Nós somos da empresa "Fast Engineering S/A" e gostaríamos de uma solução dos senhores(as), que fazem parte da empresa terceira "TI SOLUÇÕES INCRÍVEIS".

Nosso eCommerce está crescendo e a solução atual não está atendendo mais a alta demanda de acessos e compras que estamos tendo.

Atualmente usamos:

- 1 servidor para `Banco de Dados Mysql`

  - 500 GB de dados
  - 10 GB de RAM
  - 3 Core CPU

- 1 servidor com 3 APIs, com o Nginx servindo de balanceador de carga e que armazena estáticos como fotos e links - `Back-End`

  - 5 GB de dados
  - 4 GB de RAM
  - 2 Core CPU

- 1 servidor para a aplicação utilizando REACT – `Front-End`
  - 5 GB de dados
  - 2 GB de RAM
  - 1 Core CPU

<div align="center">

![image](https://github.com/user-attachments/assets/aed6046f-9050-4a13-8b6b-cad808b385ce)

<p>Diagrama da situa;cão atual</p>

</div>

Queremos modernizar esse sistema para a AWS, precisamos seguir as melhores práticas arquitetura em Cloud AWS, a nova arquitetura deve seguir as seguintes diretrizes:

- Ambiente Kubernetes;
- Banco de dados gerenciado (PaaS e Multi AZ);
- Backup de dados;
- Sistema para persistência de objetos (imagens, vídeos etc.);
- Segurança;

Porém antes da migração acontecer para a nova estrutura, precisamos fazer uma migração “lift-and-shift” ou “as-is”, o mais rápido possível, só depois que iremos promover a modificação para a nova estrutura em Kubernetes.

---

## Etapa 1 - Migração Lift-and-Shift com o AWS Application Migration Service

<div align="center">

![diagrama MGN drawio (1)](https://github.com/user-attachments/assets/a098cb0f-3724-43ea-83b3-0ab30e94fb4f)

Diagrama

</div>

1. Instalação do **AWS Replication Agent** que se conecta com **AWS Migration Service**. Solução automatizada para migrar as aplicações locais para a nuvem AWS.
2. Os **Servidores locais** e os **servidores de replicação do AWS MGN** que são executados na sub-rede da área de preparação devem se comunicar continuamente com os **endpoints do AWS MGN na porta 443** para fins de autenticação, configuração e monitoramento.
3. Quando o servidor de replicação ou de conversão é inicializado, ele se conecta a um **bucket do S3 para baixar software e arquivos de configuração**.
4. O AWS MGN usa, como padrão, **computação da EC2 do tipo t3-micro** aumentando ou diminuindo o processamento conforme necessário.
5. Conexões entre os servidores locais e os servidores de replicação são criadas por **AWS Direct Connect ou VPN** (conforme preferência). Os dados são **criptografados em trânsito** com chave de criptografia AES de 256 bits por padrão.
6. Armazenamentos da **Elastic Block Store** são criados do mesmo tamanho dos discos do sistema de origem para manter os dados do ambiente de origem **sincronizados na AWS**.
7. Para a migração do banco de dados, é usado o serviço **AWS Database Migration Service**. E o banco de dados na AWS passa a ser usado pelo **Amazon RDS**.
8. Uma das funções do servidor de replicação é emitir uma chamada de API para **tirar snapshots dos volumes de preparação do EBS** durante a replicação. Para isso, a conectividade com um endpoint da API do EC2 na porta 443 também deve estar em vigor.
9. A subnet da área de testes inicia as instâncias conforme a configuração do **modelo de execução**. Essas instâncias de testes são conectadas a **cópias recentes dos armazenamentos EBS da área de preparação**.

> [!important]
> Essas cópias não são mais sincronizadas com os servidores locais.

10. Pode ser usado uma conexão via **proxy**. É necessário quando uma empresa precisa de **auditoria do processo** ou quando **não é permitido uma conexão direta com a AWS**.
11. O **SSM** é usado para **gerenciar essas instâncias de forma automatizada**.

# Informações importantes ⬇️

### `Quais atividades são necessárias para a migração?`

1. Instalar o AWS Replication Agent para se conectar com AWS Migration Service;
2. Migrar o banco de dados com o AWS DMS;
3. Configurar o modelo de execução para as novas instâncias;
4. Testar as novas instâncias;
5. Migrar definitivamente para a AWS.

---

### `Quais as ferramentas vão ser utilizadas?`

|                                                                                                                                                                        Ferramenta |     | Descrição                                                                               |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | :-- | --------------------------------------------------------------------------------------- |
| <img src="https://cloud-icons.onemodel.app/aws/Architecture-Service-Icons_01312023/Arch_Migration-Transfer/64/Arch_AWS-Application-Migration-Service_64@5x.png" width="50"></img> | MGN | Automação para migrar as aplicações locais para a nuvem AWS.                            |
|                                                         <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRULf2JOHbvkPux8pEzQrkH70TVSpfgRMzgQA&s" width="50"></img> | EC2 | Computação para replicação dos servidores locais e para testes com servidores migrados. |
|                                                                           <img src="https://cdn.worldvectorlogo.com/logos/amazon-s3-simple-storage-service.svg" width="50"></img> | S3  | O MGN se conecta com buckets S3 para baixar softwares e arquivos de configuração.       |
|                                                                               <img src="https://cdn.worldvectorlogo.com/logos/amazon-elastic-block-store-1.svg" width="50"></img> | EBS | Para criar armazenamentos EBS idênticos aos discos dos servidores locais.               |
|                 <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQ7L7fI-Ozxh2ni9T2E7rgX_CU-VNMOpoXfwpIxYIaifUcJL_NQ0ZJi8mGHWNRdiFXmres&usqp=CAU" width="50"></img> | RDS | Para gerenciar o novo banco de dados na AWS.                                            |
|              <img src="https://cloud-icons.onemodel.app/aws/Architecture-Service-Icons_01312023/Arch_Database/64/Arch_AWS-Database-Migration-Service_64@5x.png" width="50"></img> | DMS | Para migrar o banco de dados do servidor local para o Amazon RDS.                       |

---

### `Qual o diagrama da infraestrutura na AWS?`

![Sistema pós-migração drawio (2)](https://github.com/user-attachments/assets/41208896-6f92-499f-b856-1f299e9a71a6)

Diagrama após a Migração ⬆️

---

### `Como serão garantidos os requisitos de Segurança?`

Os dados são **criptografados em trânsito** com chave de criptografia AES de 256 bits por padrão.

---

### `Como será realizado o processo de Backup?`

O sistema será distribuido em duas zonas de disponibilidade. Cada zona terá uma instância anexada com seu volume EBS sincronizado. O Banco de dados terá sua réplica na outra zona de disponibilidade.

---

### `Qual o custo da infraestrutura na AWS (AWS Calculator)?`

`Durante a migração` ⬇️

![image](https://github.com/user-attachments/assets/fbe48cd3-0d24-4556-b0ca-398be3d7786c)

Valor total por mês: **U$ 276,58**

---

`Após a migração` ⬇️

![image](https://github.com/user-attachments/assets/15d060b0-ea60-4244-bed9-66afd7e5cbb1)

Valor total por mês: **U$ 195,01**

---

## Etapa 2 - Modernização/ Kubernetes
