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
1 | Provisionamento de Ambientes (Staging Area, Homologação) + Modelagem de Dados + Definição das Regras de Negócio | 28/Dez/18
2 | Carga direta dos Dados no Ambiente de Homologação  + Validação das Regras de Negócio | 18/Jan/19
3 | Automação no Consumo dos Dados e Modelos *Staging Area* | 01/Fev/19
4 | Sanitização dos Dados | 15/Fev/19
5 | Automação da Infraestrutura e Processamento | 28/Fev/19
6 | Revisão da Infra - Ajustes Finais | 15/Mar/19



# Arquitetura da Solução

![Arquitetura (Visão Geral)](/images/img1.png)


# Tecnologias (Baseado em Software Livre)

- Framework R / Python (para Avaliação da Qualidade dos Dados e posterior Sanitização)
- Java 
- SGBD Relacional (Para Staging Area)
- Vagrant/Docker para provisionamento automático da infra-estrutura
- Ambiente Rundeck para escalonamento das rotinas
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

| Nome        | Tipo           | Tamanho  | Descrição |
| ------------- |:-------------:| -----:|:-----|
| CD_OPERADORA | varchar      |   6  |  Código de Registro da Operadora na ANS     |
| NM_OPERADORA| varchar      |   140  |  Nome fantasia ou razão social da operadora     |
| DS_CLASSIFICACAO | varchar      |   40 |  Classificação da operadora, conforme seu estatuto jurídico: - Administradora - Cooperativa Médica - Cooperativa odontológica - Autogestão - Medicina de Grupo - Odontologia de Grupo - Filantropia - Seguradora    |
| ID_ESTABELECIMENTO_SAUDE  | number      |   8  |  Código identificador do vínculo do estabelecimento de saúde junto à operadora    |
| CD_CNPJ_ESTB_SAUDE | varchar      |   14  |  CNPJ do Estabelecimento de Saude   |
| CD_CNES | varchar      |   7  |  Código identificador do estabelecimento de saúde no Cadastro Nacional de Estabelecimentos de Saúde (CNES)     |
| NM_ESTABELECIMENTO_SAUDE  | varchar      |   60  |  Nome do Estabelecimento de Saude     |
| DE_CLAS_ESTB_SAUDE | varchar      |   29  |  Classificação do estabelecimento de saúde: - Assistência Hospitalar - Serviços de Alta Complexidade - Demais Estabelecimentos - Não informado    |
| DE_TIPO_PRESTADOR  | varchar      |   50  |  Descrição do tipo de relação jurídica entre operadora e prestador: - Próprio - Contratualizado - Não informado     |
| LG_URGENCIA_EMERGENCIA  | varchar      |   8  |  Marcador que indica se o contrato entre o estabelecimento e a operadora inclui serviço de urgência/emergência (1) inclui serviço de urgência/emergência (0) não inclui serviço de urgência/emergência |
| DE_TIPO_CONTRATO | varchar      |   50  |  Descrição do tipo de relação contratual entre operadora e o estabelecimento: - Direto (diretamente contratado pela operadora) - Indireto (contratado através de outra operadora) - Não informado    |
| DE_DISPONIBILIDADE | varchar      |   13  |  Tipo de disponibilidade dos serviços do prestador: 1. Parcial (contrato vincula apenas alguns dos serviços prestados pelo estabelecimento) 2. Total (contrato vincula todos os serviços prestados pelo estabelecimento) 3. Não informado    |
| CD_MUNICIPIO | varchar      |   6  |  Código de identificação do município de localização do estabelecimento de saúde (diferencia entre municípios com o mesmo nome).     |
| NM_MUNICIPIO_X | varchar      |   50  |  Nome do município de localização do estabelecimento de saúde.     |
| SG_UF | varchar      |   2  |  Sigla da Unidade Federativa de localização do estabelecimento de saúde     |
| NM_REGIAO | varchar      |   20  |  Nome da região geográfica de localização do estabelecimento de saúde    |
| DT_VINCULO_OPERADORA_INICIO | number      |   8  |  Data e hora do início do vínculo entre operadora e estabelecimento.    |
| DT_VINCULO_OPERADORA_FIM  | number      |   8  |  Data e hora do fim do vínculo entre operadora e estabelecimento. Estabelecimentos que ainda estão com vínculo ativo não apresentam data de fim de vínculo.   |
| COMPETENCIA | varchar      |   6  |  Ano e mês da competência de referência dos dados.    |
| DATA_ATUALIZACAO | DATE      |   -  |  Data da extração dos dados no formato: DD/MM/YYYY.    |


## Características dos Produtos da Saúde Suplementar

| Nome        | Tipo           | Tamanho  | Descrição |
| ------------- |:-------------:| -----:|:-----|
ID_PLANO|NUMBER|8|Identificação única do plano (chave) |
Código do Plano|CHAR|30|Código de registro do produto na ANS |
Nome do Plano|CHAR|140|Nome do produto registrado na ANS |
Registro ANS|CHAR|6|Código de registro da operadora na ANS |
Nome Operadora|CHAR|140|Nome fantasia ou razão social da operadora |
Código Modalidade|NUMBER|8|"Código de classificação da operadora, conforme seu estatuto jurídico. (21 ou 55) Administradora (22) Cooperativa Médica (23) Cooperativa odontológica (24) Autogestão (25) Medicina de Grupo (26) Odontologia de Grupo (27) Filantropia (28 ou 29) Seguradora" |
Modalidade Operadora|CHAR|40|"Classificação da operadora, conforme seu estatuto jurídico: - Administradora - Cooperativa Médica - Cooperativa odontológica - Autogestão - Medicina de Grupo - Odontologia de Grupo - Filantropia - Seguradora" |
Porte Operadora|CHAR|50|"Classificação da operadora, conforme quantidade de beneficiários com vínculo ativo no mês mais recente disponível no SIB: - GRANDE (acima de 100 mil  vínculos de beneficiário ativos) - MÉDIA (de 20 mil a 100 mil vínculos de beneficiário ativos) - PEQUENA (abaixo de 20 mil vínculos de beneficiário ativos) - SEM BENEFICIÁRIOS" |
Vigência Plano|CHAR|1|"Início da vigência do plano para comercialização: A = Anterior à Lei 9656/1998 P = Posterior à Lei 9656/1998 ou planos adaptados à lei" |
Código Contratação|NUMBER|8|"Código do tipo de contratação do plano: (1) Individual ou familiar (2) Coletivo empresarial  (3) Coletivo por adesão  (4) Individual ou Familiar + Coletivo Empresarial  (5) Individual ou Familiar + Coletivo por Adesão  (6) Coletivo Empresarial + Coletivo por Adesão  (7) Individual + Coletivo Empresarial + Coletivo por Adesão " |
Tipo Contratação|CHAR|70|"Tipo de contratação do plano: (1) Individual ou familiar (2) Coletivo empresarial  (3) Coletivo por adesão  (4) Individual ou Familiar + Coletivo Empresarial  (5) Individual ou Familiar + Coletivo por Adesão  (6) Coletivo Empresarial + Coletivo por Adesão  (7) Individual + Coletivo Empresarial + Coletivo por Adesão " |
Contratação|CHAR|50|"Agrupamento dos tipos de contratação do plano: - INDIVIDUAL = Individual ou familiar - COLETIVO = Coletivo empresarial, Coletivo por adesão, Coletivo Empresarial + Coletivo por adesão - NÃO IDENTIFICADO = Outros tipos de contratação (planos não adaptados às mudanças de norma)" |
Código Segmentação|NUMBER|8|"Código de segmentação assistencial do plano. 01 = Ambulatorial (Ambulatorial) 02 = Hospitalar com obstetrícia (Hosp c/ obst) 03 = Hospitalar sem obstetrícia (Hosp s/ obst) 04 = Odontológico (Odontológico) 05 = Referência (Amb + hosp c/ obst) 06 = Ambulatorial + Hospitalar com obstetrícia (Amb + hosp c/ obst) 07 = Ambulatorial + Hospitalar sem obstetrícia (Amb + hosp s/ obst) 08 = Ambulatorial + Odontológico (Ambulatorial) 09 = Hosp c/ obstetrícia + Hosp s/ obstetrícia (Hosp c/s obst) 10 = Hospitalar com obstetrícia + Odontológico (Hosp c/ obst) 11 = Hospitalar sem obstetrícia + Odontológico (Hosp s/ obst) 12 = Amb + Hosp c/s Obstetrícia (Amb + hosp c/s obst) 13 = Ambulatorial + Hospitalar com obstetrícia + Odontológico (Amb + hosp c/ obst) 14 = Ambulatorial + Hospitalar sem obstetrícia + Odontológico (Amb + hosp s/ obst) 15 = Hosp c/s  Obstetrícia + Odont (Hosp c/s obst)" |
Tipo Segmentação Assistencial|CHAR|100|"Tipo de segmentação assistencial do plano: 01 = Ambulatorial (Ambulatorial) 02 = Hospitalar com obstetrícia (Hosp c/ obst) 03 = Hospitalar sem obstetrícia (Hosp s/ obst) 04 = Odontológico (Odontológico) 05 = Referência (Amb + hosp c/ obst) 06 = Ambulatorial + Hospitalar com obstetrícia (Amb + hosp c/ obst) 07 = Ambulatorial + Hospitalar sem obstetrícia (Amb + hosp s/ obst) 08 = Ambulatorial + Odontológico (Ambulatorial) 09 = Hosp c/ obstetrícia + Hosp s/ obstetrícia (Hosp c/s obst) 10 = Hospitalar com obstetrícia + Odontológico (Hosp c/ obst) 11 = Hospitalar sem obstetrícia + Odontológico (Hosp s/ obst) 12 = Amb + Hosp c/s Obstetrícia (Amb + hosp c/s obst) 13 = Ambulatorial + Hospitalar com obstetrícia + Odontológico (Amb + hosp c/ obst) 14 = Ambulatorial + Hospitalar sem obstetrícia + Odontológico (Amb + hosp s/ obst) 15 = Hosp c/s  Obstetrícia + Odont (Hosp c/s obst)" |
Segmentação Assistencial|CHAR|50|"Agrupamento dos tipos de segmentação assistencial do plano: - AMBULATORIAL (Ambulatorial, Ambulatorial + Odontológico) - AMB + HOSP C/ OBST (Ambulatorial + Hospitalar com obstetrícia, Ambulatorial + Hospitalar com obstetrícia + Odontológico, Referência) - AMB + HOSP S/ OBST (Ambulatorial + Hospitalar sem obstetrícia , Ambulatorial + Hospitalar sem obstetrícia + Odontológico) - AMB + HOSP C/S OBST (Amb + Hosp c/s Obstetrícia) - HOSP C/ OBST (Hospitalar com obstetrícia, Hospitalar com obstetrícia + Odontológico) - HOSP S/ OBST (Hospitalar sem obstetrícia, Hospitalar sem obstetrícia + Odontológico) - HOSP C/S OBST (Hosp c/ obstetrícia + Hosp s/ obstetrícia, Hosp c/s  Obstetrícia + Odont ) - ODONTOLÓGICO (Odontológico)" |
Cobertura|CHAR|50|"Categorias de cobertura de produto por tipo de segmentação assistencial e modalidade da operadora: - Médico-hospitalar (cobertura médico-hospitalare pode incluir cobertura odontológica) - Odontológico (exclusivamente odontológico)" |
Tipo Financiamento|CHAR|30|"Modalidade de financiamento do plano: - Pós-estabelecido - Pre-estabelecido - Misto- (vazio) = Modalidade de financiamento não informada" |
Código Abrangência Cobertura|NUMBER|8|"Código das categorias de área de cobertura do plano. (1) Nacional (2) Grupo de estados (3) Estadual (4) Grupo de municípios (5) Municipal (6) Outras" |
Abrangência Cobertura|CHAR|60|"Categorias de área de cobertura do plano. (1) Nacional (2) Grupo de estados (3) Estadual (4) Grupo de municípios (5) Municipal (6) Outras" |
Fator Moderador|CHAR|50|"Tipo de fator moderador (mecanismo de regulação de utilização) do plano: - Co-participacão - Franquia - Franquia + Co-participacão - Ausente" |
Log Fator Moderador|NUMBER|3|"Indicador de presença de fator moderador.  1 = Presença de algum tipo de fator moderador 0 = Fator moderador ausente" |
Acomodação Hospitalar|CHAR|14|"Tipo de acomodação do plano  com internação: - Coletiva - Individual - Sem Acomodação (planos ambulatoriais ou odontológicos) - Não identificado (planos com internação e sem tipo de acomodação informada)" |
Situação Plano|CHAR|60|"Situação de comercialização do plano: - ATIVO - ATIVO COM COMERCIALIZAÇÃO SUSPENSA - CANCELADO" |
Data Situação Plano|NUMBER|8|Data e hora da última atualização de situação do plano (dd/mm/aaaa hh:mm) |
Data Registro Plano|NUMBER|8|Data e hora do registro do plano junto à ANS (dd/mm/aaaa hh:mm) |


## Abrangência Geográfica dos Planos de Saúde

| Nome        | Tipo           | Tamanho  | Descrição |
| ------------- |:-------------:| -----:|:-----|
| ID_PLANO | number      |   8  |  Identificação única do plano (chave)     |
| CD_PLANO | varchar      |   30  |  Código de registro do produto na ANS     |
| CD_OPERADORA | varchar      |   6  |  Código de registro da operadora na ANS    |
| CD_MUNICIPIO | varchar      |   6  |  Código identificador do município de cobertura do plano     |
| NM_MUNICIPIO_X | varchar      |   50  |  Nome do município de cobertura do plano     |
| SG_UF | varchar      |   2  |  Sigla da Unidade Federativa    |
| NM_UF | varchar      |   20  |  Nome da Unidade Federativa     |
| REGIAO_GEOGRAFICA | varchar      |   20  |  Nome da Região Geográfica     |


## Histórico de Planos de Saúde

| Nome        | Tipo           | Tamanho  | Descrição |
| ------------- |:-------------:| -----:|:-----|
| ID_PLANO | number      |   8  |  Identificação única do plano (chave)     |
| CD_PLANO | varchar      |   30  |  Código de registro do produto na ANS     |
| CD_OPERADORA | number      |   8  |  Código de registro da operadora na ANS    |
| DT_INICIO_STATUS | varchar      |   8  |  Data de início da situação do plano    |
| DT_FIM_STATUS | varchar      |   8  |  Data de fim da situação do plano (vazio indica a situação atual do plano)    |
| ID_SITUACAO_PRINCIPAL | varchar      |   8  |  Código da situação principal do plano    |
| DE_SITUACAO_PRINCIPAL | varchar      |   60  |  Descrição da situação principal do plano: 1 ATIVO 2 ATIVO COM COMERCIALIZAÇÃO SUSPENSA 3 CANCELADO 4 TRANSFERIDO    |
