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
    - É uma rede de datacenters que são espalhados pelo mundo que são pontos de presença
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
      - Serviços com o ambiente pré definido pela AWS, ou seja, a resposabilidade se volta pra cima do cloud provider
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
    - Cada chamada pra AWS ela valida a requisição com um algortimo interno pra fazer a autenticação em que impede ataques de heap play (sigv4)
  - Principio do privilégio minimo
  - Rotacionamento de chaves
  - AWS Cloud Trail
  - AWS Organizations
  - Proteção do usuário root
  - IAM
    - Criação de usuários
    - Integração com outros sistemas de identidade
    - Controle multi-factor autentication (MFA)
    - Permissão no nível de arquivo (granularidade de permissionamento)