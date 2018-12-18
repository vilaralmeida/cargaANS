# Carga de Dados Abertos da ANS
Solução de Carga de Dados Abertos da Agência Nacional de Saúde Suplementar

# Introdução

A Carga de Dados ANS é uma solução que consolida dados de várias fontes de dados em um único ambiente.

Para realização da Carga ANS, os seguintes desafios devem ser enfrentados:

- Consumo automático dos dados provisionados em Portais de Dados abertos e plataformas semelhantes;
- Consolidação dos Dados de vários tamanhos, formatos e fontes em um único ambiente intermediário;
- Sanitização e preparação  dos dados para inserção dos dados em ambiente de homologação;
- Relatório de Homologação para garantir que a carga dos dados está sendo respeitada e não há perda da dados;
- Carga dos dados em Ambiente de Produção.


# Proposta de Cronograma

Sprint | Escopo | Prazo
--- | --- | ---
1 | Carga direta dos Dados no Ambiente de Homologação | 28/Dez
2 | Automação no Consumo dos Dados e Provisionamento Ambiente *Staging Area* | 18/Jan
3 | Sanitização dos Dados | 01/Fev


# Arquitetura da Solução

![Arquitetura (Visão Geral)](/images/img1.png)


# Tecnologias (Baseado em Software Livre)

- Framework R / Python (para consumo dos dados em várias fontes e consolidação no ambiente)
- Java para automação da Carga da Staging Area
- SGBD Relacional (Para Staging Area)
- Vagrant/Docker para provisionamento automático da infra-estrutura
- Ambiente Nuvem para construção das etapas intermediárias do Processo
- Documentação completa disponível no Github



# Fontes de Dados (em construção)

- ANS/RPS - Produtos e Prestadores Hospitalares
- ANS/RPS - Operadoras e prestadores não-hospitalares
- ANS/RPS - Características dos Produtos da Saúde Suplementar
- ANS/RPS - Abrangência Geográfica dos Planos de Saúde
- ANS/RPS - Histórico de Planos de Saúde


# Dicionário de Dados

## Produtos e Prestadores Hospitalares

| Nome        | Tipo           | Tamanho  | Descrição |
| ------------- |:-------------:| -----:|:-----|
| ID_REDE      | varchar | 81 |   Identificação única de vínculo do prestador de serviço na rede assistencial do plano de saúde (chave)    | 
| CD_OPERADORA      | varchar      |   6  |   Código de registro da operadora na ANS    |
| NO_RAZAO | varchar      |   140  |  Razão Social da Operadora     |
| DS_CLASSIFICACAO | varchar      |   40  |  Classificação da operadora, conforme seu estatuto jurídico: 1. Administradora 2. Cooperativa Médica 3. Cooperativa odontológica 4. Autogestão 5. Medicina de Grupo 6. Odontologia de Grupo - Filantropia 7. Seguradora    |
| DE_PORTE | varchar      |   17  |  Classificação da operadora, conforme quantidade de beneficiários com vínculo ativo no mês mais recente disponível no SIB: - GRANDE (acima de 100 mil vínculos de beneficiário ativos) 1. MÉDIA (de 20 mil a 100 mil vínculos de beneficiário ativos) 2. PEQUENA (abaixo de 20 mil vínculos de beneficiário ativos) 3. SEM BENEFICIÁRIOS    |
| ID_PLANO | number      |   8  |  Identificação única do plano (chave)     |
| CD_PLANO | varchar      |   20  |  Código de registro do produto na ANS    |
| TP_VIGENCIA_PLANO | varchar      |   1  |  Início da vigência do plano para comercialização: 1. A = Anterior à Lei 9656/1998  2. P = Posterior à Lei 9656/1998 ou planos adaptados à lei     |    
| CONTRATACAO | varchar      |   16  |  Agrupamento dos tipos de contratação do plano: 1. INDIVIDUAL = Individual ou familiar 2. COLETIVO = Coletivo empresarial, Coletivo por adesão, Coletivo Empresarial + Coletivo por adesão 3. NÃO IDENTIFICADO = Outros tipos de contratação (planos não adaptados às mudanças de norma)     |
| DE_TIPO_CONTRATACAO | varchar      |   70  |  Tipo de contratação do plano: (1) Individual ou familiar (2) Coletivo empresarial (3) Coletivo por adesão (4) Individual ou Familiar + Coletivo Empresarial (5) Individual ou Familiar + Coletivo por Adesão (6) Coletivo Empresarial + Coletivo por Adesão (7) Individual + Coletivo Empresarial + Coletivo por Adesão    |
| DE_TIPO_MODALIDADE_FINM | varchar      |   30  |  Modalidade de financiamento do plano: 1. Pós-estabelecido 2. Pre-estabelecido 3. Misto 4. (vazio) = Modalidade de financiamento não informada    |
| SEGMENTACAO_ASSISTENCIAL | varchar      |   100  |  Agrupamento dos tipos de segmentação assistencial do plano: 1. AMBULATORIAL (Ambulatorial, Ambulatorial + Odontológico) 2. AMB + HOSP C/ OBST (Ambulatorial + Hospitalar com obstetrícia, Ambulatorial + Hospitalar com obstetrícia + Odontológico, Referência) - AMB + HOSP S/ OBST (Ambulatorial + Hospitalar sem obstetrícia , Ambulatorial + Hospitalar sem obstetrícia + Odontológico) 3. AMB + HOSP C/S OBST (Amb + Hosp c/s Obstetrícia) 4. HOSP C/ OBST (Hospitalar com obstetrícia, Hospitalar com obstetrícia + Odontológico) 5. HOSP S/ OBST (Hospitalar sem obstetrícia, Hospitalar sem obstetrícia + Odontológico) 6. HOSP C/S OBST (Hosp c/ obstetrícia + Hosp s/ obstetrícia, Hosp c/s Obstetrícia + Odont ) 7. ODONTOLÓGICO (Odontológico)     |
| DE_TIPO_ABRANGENCIA_GEOGRAFICA | varchar      |   60  |  Categorias de área de cobertura do plano. 1. Nacional 2. Grupo de estados 3. Estadual 4. Grupo de municípios (5) Municipal 5. Outras     |
| LG_FATOR_MODERADOR | Number      |   8  |  Indicador de presença de fator moderador. 1 = Presença de algum tipo de fator moderador 0 = Fator moderador ausente     |
| DE_SITUACAO_PRINCIPAL | varchar      |   60  |  Situação de comercialização do plano: 1. ATIVO 2. ATIVO COM COMERCIALIZAÇÃO SUSPENSA 3. CANCELADO     |
| ID_ESTABELECIMENTO_SAUDE | number      |   8  |  Código identificador do vínculo do estabelecimento de saúde junto à operadora     |
| CD_CNPJ_ESTB_SAUDE | varchar      |   14  |  CNPJ do estabelecimento de saúde     |
| CD_CNES | varchar      |   7 |  Código identificador do estabelecimento de saúde no Cadastro Nacional de Estabelecimentos de Saúde (CNES)     |
| NM_ESTABELECIMENTO_SAUDE | varchar      |   60  |  Nome do estabelecimento de saúde.    |
| DE_CLAS_ESTB_SAUDE | varchar      |   29  |  Classificação do estabelecimento de saúde: 1. Assistência Hospitalar 2. Serviços de Alta Complexidade 3. Demais Estabelecimentos 4. Não informado     |
| LG_URGENCIA_EMERGENCIA | number      |   8  |  Marcador que indica se o contrato entre o estabelecimento e a operadora inclui serviço de urgência/emergência (1) inclui serviço de urgência/emergência (0) não inclui serviço de urgência/emergência    |
| DE_TIPO_PRESTADOR | varchar      |   15  |  Descrição do tipo de relação jurídica entre operadora e prestador: 1. Próprio (estabelecimento pertence à operadora) 2. Contratualizado (vinculado por contrato de prestação de serviço) 3. Não informado    |
| DE_TIPO_CONTRATO | varchar      |   13  |  Descrição do tipo de relação contratual entre operadora e o estabelecimento: 1. Direto (diretamente contratado pela operadora) 2. Indireto (contratado através de outra operadora) 3. Não informado     |
| DE_DISPONIBILIDADE | varchar      |   13  |  Tipo de disponibilidade dos serviços do prestador: 1. Parcial (contrato vincula apenas alguns dos serviços prestados pelo estabelecimento) 2. Total (contrato vincula todos os serviços prestados pelo estabelecimento) 3. Não informado    |
| CD_MUNICIPIO | varchar      |   6  |  Código de identificação do município de localização do estabelecimento de saúde (diferencia entre municípios com o mesmo nome).     |
| NM_MUNICIPIO_X | varchar      |   50  |  Nome do município de localização do estabelecimento de saúde.     |
| SG_UF | varchar      |   2  |  Sigla da Unidade Federativa de localização do estabelecimento de saúde     |
| DT_VINCULO_INICIO | number      |   8  |  Data de início do vínculo do prestador de serviço com o plano     |
| DT_VINCULO_FIM | number      |   8  |  Data de fim do vínculo do prestador de serviço com o plano. Estabelecimentos que ainda estão com vínculo ativo não apresentam data de fim de vínculo.     |
| NM_REGIAO | varchar      |   20  |  Nome da região geográfica de localização do estabelecimento de saúde    |


## Produtos e Prestadores não Hospitalares


## Características dos Produtos da Saúde Suplementar


## Abrangência Geográfica dos Planos de Saúde

## Histórico de Planos de Saúde
