# Design e Arquitetura de Software II 2025/2

[AWS CANVAS](https://awsacademy.instructure.com/courses/129676)

## 30/07

- Well Architect Framework
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