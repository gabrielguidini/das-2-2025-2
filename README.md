# Design e Arquitetura de Software II 2025/2

[AWS CANVAS](https://awsacademy.instructure.com/courses/129676)

## 30/07

- Well Architect Framework
  - Documento para seguir boas práticas para a AWS
- Security Pilar
- Cost Optimization Pilar
- Sustainability Pilar Don't waste
- EC2 Autoscaling
  - In
  - Out
- Observabilidade
  - Cloud Watch - logs
- Trade-offs

## 06/08

- Global Infraestructure (AZ -> conjunto de data centers)
  - Availiability Zone:
    - Região é uma área geográfica com um conjunto de data centers da AWS
    - Cada região possuí duas ou mais Availability Zones
    - A comunicação entre regiões usam a infrasestrutura AWS backbone network (rede proprietária)
    - Distribuição de serviços entre AZ's 1 ou mais data centers
      - AZ's são tolerantes a falhas
      - High Availability -> Alta Disponibilidade 
  - Local Zone:
    - Locais que não são grandes o suficientes para que seja implementado AZ's
    - Para esses locais que não são justificáveis a implementação de AZ's
    - O acesso a uma LZ tem como "pai" uma AZ, mas que na conexão não irá até a AZ
  - Wavelength:
    - Estrutura da AWS pra rodar servidor perto de antenas 5G (empresas de telefonia)
  - Data Center:
    - Gerenciados somente por funcionários da AWS
  - AWS PoPs (CDN -> Cloud Front)
    - É uma rede de datacenters espalhados pelo mundo que são pontos de presença
    - Edge Locations
      - Data centers e servidores que são localizados perto do consumidor
    - Regional Edge Cache
      - Estrutura centralizadora de dados relacionados a necessidade do dado, caso não tenha no regional edge cache pede para a região o dado principal
- Security Pillar
  - Shared Responsibility Model
    - Demonstra a responsabilidade entre Customer e AWS, o compartilhamento das responsabilidades
    - IaaS -> Infraestructure as a Service
      - Rede, computadores, parte física -> provida pela AWS
    - PaaS -> Plataform as a Service
      - Serviços com o ambiente pré-definido pela AWS, ou seja, a resposabilidade se volta pra cima do cloud provider
    - SaaS -> Software as a Service
      - Office 365, DocWorks, PowerBI, QuickSite
      - Dados e usuários, o resto será feito e gerenciado pela AWS
  - Principais pontos de atenção na segurança:
    - Criação de um sistema forte de indentidade forte
      - Permissionamento de acesso aos recursos, em a AWS possuí um serviço que descreve as policies (IAM - Identity and Access Management), todo processo de segunraça e identidade nos serviços da AWS, autenticação e identificação do usuário
    - Proteção de dados em trânsito e em descanso
      - Client-side encryption -> antes da mensagem sair é criptografado e é descriptografado quando chega do outro lado
      - Server-side encryption -> sai do cliente criptografado chega descriptografado na AWS mas quando é para armazenar o dado, ele é criptografado pela AWS novamente
    - Aplicação de segurança em todas as camadas
    - Determinar o nível de acesso de um funcionário somente o necessário
    - Manter o rastreabilidade
    - Preparar para problemas de segurança
    - Melhores práticas automação de segurança
  - Authentication And Securing Access
    - Autenticação X Autorização (IAM)
      - Autenticação:
        - O que tem, O que vc sabe, O que vc é
      - Autorização:
        - Allow ou Deny 
    - Cada chamada para AWS ela valida a requisição com um algortimo interno pra fazer a autenticação em que impede ataques de heap play (sigv4)
  - Principio do privilégio minimo
  - Rotacionamento de chaves
  - AWS Cloud Trail
  - AWS Organizations
  - Proteção do usuário root
  - IAM
    - Criação de usuários
      - Para cada tipo de necessidade
    - Integração com outros sistemas de identidade
    - Controle multi-factor autentication (MFA)
      - Para todos os tipos de usuários
    - Permissão no nível de arquivo (granularidade de permissionamento)

## 13/08 - Continuação Security

  - Permissionamento
    - Identity Based Policy
      - Permissão a nível de usuário, grupos ou roles dentro do IAM
    - Resource Based Policy
      - Permissão a nível de serviços da AWS
    - A diferença no JSON seria a parte de determinar qual o user tem acesso ao permissionamento `"Principal": "*"`
  - `"NotResource"` -> NEGAÇÃO DO RECURSO, ou seja, diz que não tem acesso a outros serviços tirando os que tiverem dentro desse campo

## 20/08 - Storage Layer com S3

  - Simples Storage Service (S3):
    - 3 tipos de armazenamento de dados:
      1. Block Storage ⇾ dados são armazenados num dispositivo e o sistema fraciona o disco em sistemas menores
      2. File Storage (Share) ⇾ dados são armazenados em arquivos hierarquicos (NFS/SMB) compartilhamento de disco, trocando arquivos através dele
      3. Object Storage ⇾ dados são armazenados baseados em requisições subindo via `WEB` binário + metadados
    - Não existe provisionamento desse serviço, é virtualmente ilimitado
      - Número de arquivos ilimitados
      - 5 TB máximo por objeto único
      - Os objetos recebem uma URL pública, mas não exposta para internet
      - URL única por conta que o nome do container está dentro da URL pública do serviço
    - Sistema de prefixo ⇾ vira parte da URL bucket-name + prefix + file name
    - Resiliência
    - Disponibilidade
    - Alta performace
    - Multipart upload ⇾ melhor throughtput, somente pelo CLI
      - TCP / BGP ⇾ protocolo de transporte via internet, acha o menor caminho até o destino
      - Transfer acceleration ⇾ conecta até um Cloud Front edge location (CDN) e a partir da rede interna da aws
      faz o upload para o s3
    - Storage Classes ⇾ opções de armanezamento dos arquivos (custo e disponibilidade)
  - Versioning no S3
    - Não vem habilitado por padrão, uma vez habilitado, não pode desabilitar
    - Custa a mais os valores, por duplicar, triplicar etc
    - Só pode pausar o versionamento
  - CORS (Cross-origin resource sharing)
    - Validação do browser contra pishing
    - Protege quando o site falso chame um site verdadeiro bb1.com.br ⇾ bb.com.br
    - Relação de confiança entre front-back
    - Configuração de domínio que podem acessar
  - Por padrão, somente o criador do bucket que pode acessar ele
    - Acesso configurável (via policy), podendo ser publico na internet

## 03/09 - Compute Cloud (EC2)

  - **De cima para baixo ⇾ maior gestão do cliente em cima dos processos**
    - Elastic Compute Cloud (EC2) ⇾ "VM" e "BareMetal"
      - Escala verticalmente
      - Rápida provisão
      - Hypervison abstrai uma camada de sistema operacional (XEN) para que
      consiga rodar diversos SOs nos servidores
      - On-demand ($$$), Reserved Instance ($$) e Spot-instance ($) `máquinas paradas`
      - 
    - EKS e ECS ⇾ Containers
    - LighSail ⇾ Virtual Private Servers (VPS)
    - Elastic Beanstalk ⇾ PaaS
    - Lambda e Fargate ⇾ Serverless
  - AMI
  - Compute Optimizer

## 10/09 - Continuação EC2

  - Instance Store
    - HD fisico colado na placa-mãe do servidor
    - HD não pesistente, ou seja, se der stop na máquina ou terminate
    perde todos os arquivos que estavam dentro do HD/SSD
    - Bom para buffers, cache
  - Amazon EBS ⇾ Elastic Block Storage
    - Persiste
    - Mover volume de uma máquina para outr 
    - HD ⇾ SSD / sem necessitar de derrubar o servidor
    - General Propose(gp2) ⇾ SSD
    - Indicador de saúde ⇾ AWS notifica o usuário
      - Backup ⇾ monitoramento de saúde dos HDs
    - Associação 1:1, um volume ⇾ uma instância
    - FileShare para EC2
      - EBS não É um FileShare
      - S3 não é um FileShare
      - Amazon EFS (Elastic File System) e Amazon FSx 
      - EFS:
        - Somente em SO linux, utiliza NFSv4 e ainda não é suportado
        pelo windows server.
        - Escala automaticamente para suportar a quantidade necessária de
        memória
        - Leitura e Escrita no mesmo disco
        - Copia em 3 AZs diferentes (mesma durabilidade do S3)
        - Cria 3 redes em cada uma das AZs (replicação automática)
      - Amazon FSx ⇾ FileShare Multi Proposito
        - multi propostito:
          - fsx for windows
          - fsx for netapp
          - fsx for openzfs
            - fsx for luster HPC
        - Não é elástico automaticamente

## 17/09 - Creating a network environment

  - AMI -> Basic, Silver, Golden (imagens de máquinas pré configuradas)
  - Placement group
  - Cluster -> Mesmo DataCenter, mesmo rack físico, alta performance
    - Máquinas espelhadas
  - Precificação
    - On-demand
    - Reserved Instances (reservar x instancias por x tempo)
    - Saving Plans 
    - Spot instnaces (usa máquinas ociosas, mas tem menos disponibiliade)
  - VPC
    - subnet está numa AZ e a VPC numa região
    - Peering conection

## 24/09 - Continuação networking

  - VPC
    - Subnet
      - Privada (FileShare, DB etc)
        - Totalmente isolada
        - NAT (Network Address Translator) Gateway
          - EC2(Subnet Privada) -> private subnet route 
          table -> NAT Gateway (Subnet publica) -> public subnet route table -> internet gateway
      - Publica
        - Conecta com ‘internet’ via `internet gateway` (NAT Gateway)
        - Necessita de um IP publico (elastic IP - ip elástico)
  - Firewall
    - Porta ⇾ Recurso do sistema operacional que permite que um programa faça requisições para ‘internet’ 
    - Proteção de rede
    - Network Access Control List (NACL)
      - Estatico e stateless
    - Security Group
      - Taggeia os pacotes, caso a conexão seja feita
      - Statefull

## 01/10 - Continuação network

  - Peering não é transitivo

## 08/10 - Securing User, Application and Data Access

  - Athena = SQL no S3
  - Kinesis = não é o kafka mas podia ser (Streaming da AWS)
  - MSK ⇾ Kafka da gerenciado pela AWS
  - RBAC (Role Based Access Control) x ABAC (Attribute Based Access Control)
    - RBAC
      - IAM ⇾ Policy's
      - Permissionamento baseado na role do usuário
      - User Group ⇾ padronização de permissionamento ⇾ herda permissão (permissionamento dado ao grupo),
      um usuário pode estar em mais de um user group
    - ABAC
      - Além de associar permissionamento ao grupo, a permissão só vale se
      o recurso da nuvem que estiver sendo utilizado somente se tiver um atributo em específico
      - "Somente os servidores que tiverem a tag com chave valor OWNER:XPTO"
      - Ou seja, o usuário precisa estar dentro da listagem de usuários dentro das tags
  - Identidade Federada (Usuários federados)
    - Login via Google
    - IDP (Identity provider) ⇾ empresa terceira é responsável pela autenticação do usuário
      - OpenID, SAML (Security Assertion Markup Language)
    - Identity Center ⇾ antigo SSO
    - Security Token Service (STS)
      - Esse carinha possibilita que faça um user assumir uma role/credencial temporaria
      - access-key, token-session
    - AWS Cognito
      - IDP da B2C ⇾ Business to customer
      - Integração com o cognito, liberando o login via, google, meta etc ou SAML
      - Autenticação, autorização e gerenciamento dos usuários
    - AWS Organization
      - Conta Root ⇾ Habilita o Organization
        - Invite para as filhas
        - Billing centralizado
        - Desconto por volume
        - Criação de unidade organizacionais (dev, hml, prd etc)
      - Governança
        - SCP's (Service Control Policies)
        - Sobrescreve o acesso que o IAM deu
        - Somente tira permissionamento
        - Pode colocar em:
          - Contas
          - Unidade Organizacional
          - Root (não usar aqui normalmente)
      - Control Tower ⇾ PCI Cartão de crédito
  - Criptografia Simétrica x Assimétrica
    - Simétrica
      - Chave única (tranca e destranca a informação)
    - Assimétrica
      - Public key / Private Key
  - Criptografia Server-side
  - S3SSE-KMS
    - Criptografia e descriptografia do Key Managment Server
    - Data Key nunca sai do KMS

## 15/10 - Cloud Watch

## 21/10 - 

## 29/10 - Caching content

  - CloudFront
    - CDN (Content delivery network)
    - Diminuição da distância entre cliente - servidor
      - Edge Location (cache regional) -> Cache missing -> CloudFront Regional
      -> Cache Missing -> Servidor (para recuperar a info que não estava cacheado) 
      - O mesmo conteúdo vai estar disponível na 1° Edge Location
  - Elastic Cache
    - Database layer cache - acelerar consulta no banco de dados
    - Redução de custo do banco de dados
    - Memcached
    - Redis




