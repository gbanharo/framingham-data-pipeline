# Framingham Data Pipeline

Pipeline de dados desenvolvido no Databricks utilizando PySpark e arquitetura em camadas Bronze, Silver e Gold, a partir do dataset Framingham Heart Study.

O projeto contempla a ingestão dos dados brutos, tratamento e padronização, persistência das camadas, verificações de qualidade e análise final para responder às perguntas de negócio propostas.

---

## 1. Contexto de Negócio e Perguntas

O conjunto de dados utilizado contém informações demográficas, comportamentais e clínicas relacionadas ao risco cardiovascular. A variável-alvo `TenYearCHD` indica a ocorrência de doença coronariana em um horizonte de dez anos.

As perguntas de negócio utilizadas para orientar a análise estão apresentadas na **Seção 1.3 do notebook** e investigam:

- a ocorrência de doença coronariana por faixa etária;
- a relação entre hipertensão e ocorrência de doença coronariana;
- diferenças nos indicadores clínicos entre pacientes com e sem ocorrência;
- a relação entre tabagismo e ocorrência de doença coronariana;
- a ocorrência de doença coronariana entre pacientes com e sem diabetes.

A exploração inicial dos dados e de sua estrutura está documentada nas **Seções 2 e 3 do notebook**.


---

## 2. Carga dos Dados

A ingestão dos dados foi realizada no Databricks utilizando PySpark. O dataset original foi carregado para o ambiente de processamento e utilizado como origem para a construção do pipeline.

A etapa de carregamento e inspeção inicial está documentada na **Seção 3.1 do notebook**.

### Evidência da carga e visualização inicial dos dados


<img width="831" height="290" alt="image" src="https://github.com/user-attachments/assets/4bf0f509-7374-40d0-b6fc-456957a61cfb" />


---

## 3. Modelagem e Catálogo de Dados

O pipeline foi estruturado seguindo a arquitetura em camadas **Bronze, Silver e Gold**, permitindo separar as diferentes etapas de processamento dos dados.

A organização adotada é composta por:

- **Bronze:** camada responsável por preservar os dados provenientes da ingestão inicial, mantendo uma representação próxima à fonte original;
- **Silver:** camada destinada ao tratamento, padronização e validação dos dados;
- **Gold:** camada que disponibiliza o conjunto final preparado para consumo analítico, resposta às perguntas de negócio e etapas posteriores de modelagem.

No Databricks, as três camadas foram persistidas como tabelas Delta e registradas no **Unity Catalog**, dentro do catálogo `workspace` e do schema `framingham_mvp`, com os nomes:

- `bronze_framingham`
- `silver_framingham`
- `gold_framingham`

A construção da camada Bronze está documentada na **Seção 3 do notebook**, a transformação e validação da camada Silver na **Seção 4** e a preparação e persistência da camada Gold na **Seção 5**.

### Evidência do catálogo de dados no Unity Catalog

<img width="520" height="328" alt="image" src="https://github.com/user-attachments/assets/498600d5-2e3c-4a96-8e4a-b7ab9793a596" />

A visualização do Unity Catalog confirma a existência das três tabelas que compõem o pipeline, correspondentes às camadas Bronze, Silver e Gold.

---

## 4. Pipeline de Dados

O processo de ETL foi desenvolvido integralmente no Databricks utilizando PySpark e organizado em um único notebook, seguindo de forma sequencial a construção das camadas Bronze, Silver e Gold.

A **Seção 3 do notebook** realiza a ingestão e a persistência dos dados na camada Bronze. A **Seção 4** utiliza a Bronze como origem, aplica os tratamentos e validações necessários e persiste o resultado na camada Silver. Em seguida, a **Seção 5** utiliza os dados tratados da Silver para preparar e persistir a camada Gold, utilizada nas análises finais.

Após a persistência, as tabelas Silver e Gold foram novamente carregadas a partir do Unity Catalog para validação, confirmando que o processo de gravação foi realizado corretamente.

O código completo do pipeline, incluindo as transformações, persistências e validações, está disponível no notebook publicado neste repositório.

### Evidências da execução e persistência do pipeline

A persistência da camada Silver é realizada na **Seção 4.3 do notebook**:

<img width="302" height="185" alt="image" src="https://github.com/user-attachments/assets/44930f5e-a767-492f-97d8-608c49f5ea8b" />

A persistência da camada Gold é realizada na **Seção 5.7 do notebook**:

<img width="334" height="185" alt="image" src="https://github.com/user-attachments/assets/d574e49e-6e45-4c24-a6f4-cfc0bebe88c5" />

Após a gravação, as tabelas Silver e Gold foram novamente carregadas a partir do Unity Catalog e validadas nas etapas posteriores do notebook, verificando a quantidade de registros e atributos, além da presença de registros duplicados e valores ausentes.

---

## 5. Qualidade dos Dados

A qualidade dos dados foi verificada ao longo das etapas de construção do pipeline. Após os tratamentos realizados na camada Silver e a preparação da camada Gold, o conjunto final persistido foi submetido a uma nova validação.

Na **Seção 5.8 do notebook**, a tabela Gold foi carregada novamente a partir do Unity Catalog para verificar se os dados haviam sido armazenados corretamente. Foram conferidos a quantidade de registros e atributos, a presença de registros duplicados e a existência de valores ausentes.

A validação apresentou os seguintes resultados:

- **4.238 registros**;
- **16 atributos**;
- **0 registros duplicados**;
- **0 valores ausentes**.

### Evidência da validação da camada Gold

<img width="363" height="441" alt="image" src="https://github.com/user-attachments/assets/a042ec83-cbb8-4995-b5e7-f90022eea293" />

Posteriormente, a **Seção 6 do notebook** complementa a avaliação da qualidade dos dados por meio da verificação dos valores e domínios das variáveis, análise dos intervalos numéricos e inspeção de valores extremos. Os valores de maior magnitude identificados foram mantidos quando não havia evidências suficientes para classificá-los como erros.

Dessa forma, a validação da camada Gold e as verificações complementares da Seção 6 indicam que o conjunto final apresenta condições adequadas para a realização das análises propostas no projeto.

---


## 6. Análise de Dados e Respostas às Perguntas de Negócio

A análise final dos dados está apresentada na **Seção 7 do notebook** e foi realizada a partir dos dados tratados e disponibilizados na camada Gold. O objetivo desta etapa foi responder às **cinco perguntas de negócio definidas no início do projeto**, utilizando as informações disponíveis no conjunto de dados Framingham.

As análises foram organizadas da seguinte forma:

- **Seção 7.1:** ocorrência de doença coronariana por faixa etária;
- **Seção 7.2:** hipertensão e ocorrência de doença coronariana;
- **Seção 7.3:** indicadores clínicos e ocorrência de doença coronariana;
- **Seção 7.4:** tabagismo e ocorrência de doença coronariana;
- **Seção 7.5:** diabetes e ocorrência de doença coronariana;
- **Seção 7.6:** síntese das respostas às perguntas de negócio.

### Principais resultados

As **cinco perguntas de negócio propostas foram respondidas** a partir das análises realizadas sobre os dados da camada Gold.

Em relação à **idade**, a ocorrência de `TenYearCHD = 1` aumentou entre as faixas etárias analisadas, passando de **4,14% entre indivíduos de 30 a 39 anos para 27,68% entre aqueles de 60 a 70 anos**.

Na análise da **hipertensão**, a ocorrência de `TenYearCHD = 1` foi de **10,92% entre indivíduos sem hipertensão** e de **24,70% entre indivíduos com hipertensão**.

Os **indicadores clínicos** também apresentaram diferenças entre os grupos. Os indivíduos com `TenYearCHD = 1` apresentaram, em média, valores mais elevados de colesterol total, pressão arterial sistólica e diastólica, IMC e glicose.

Em relação ao **tabagismo**, a ocorrência de `TenYearCHD = 1` foi de **14,51% entre não fumantes atuais** e de **15,90% entre fumantes atuais**. Além disso, os indivíduos com `TenYearCHD = 1` apresentaram maior média de cigarros consumidos por dia.

Na análise da **diabetes**, **14,63% dos indivíduos sem diabetes** apresentaram `TenYearCHD = 1`, enquanto entre os indivíduos com diabetes esse percentual foi de **36,70%**. Esse resultado deve ser interpretado considerando que o grupo com diabetes possui um número menor de registros no conjunto de dados.

De forma geral, os resultados permitiram identificar diferenças na ocorrência de doença coronariana entre os grupos analisados e **responder às cinco perguntas de negócio definidas para o projeto**. As associações identificadas possuem caráter descritivo e não devem ser interpretadas como relações de causa e efeito.

### Evidências das respostas às perguntas de negócio

A seguir são apresentadas as evidências obtidas no Databricks para cada uma das cinco perguntas de negócio.

#### 6.1 Ocorrência de doença coronariana por faixa etária

A análise correspondente está apresentada na **Seção 7.1 do notebook**.

<img width="347" height="478" alt="image" src="https://github.com/user-attachments/assets/eb7c9920-d3fc-4144-b852-fd30f68d2b18" />

#### 6.2 Hipertensão e ocorrência de doença coronariana

A análise correspondente está apresentada na **Seção 7.2 do notebook**.

<img width="396" height="311" alt="image" src="https://github.com/user-attachments/assets/a480bc7d-e9d3-4760-9df7-b9bbe5b4543c" />

#### 6.3 Indicadores clínicos e ocorrência de doença coronariana

A análise correspondente está apresentada na **Seção 7.3 do notebook**.

<img width="491" height="251" alt="image" src="https://github.com/user-attachments/assets/8dea36de-41da-4aad-bbe8-4ce7f310623b" />

#### 6.4 Tabagismo e ocorrência de doença coronariana

A análise correspondente está apresentada na **Seção 7.4 do notebook**.

<img width="403" height="319" alt="image" src="https://github.com/user-attachments/assets/cad48c96-d299-43bf-919e-0e370bccbee1" />

#### 6.5 Diabetes e ocorrência de doença coronariana

A análise correspondente está apresentada na **Seção 7.5 do notebook**.

<img width="377" height="317" alt="image" src="https://github.com/user-attachments/assets/2a2a3a84-f775-471f-81a3-d0e3f6b6153c" />

As evidências apresentadas demonstram a execução das cinco análises no Databricks e os resultados utilizados para responder às perguntas de negócio propostas. A consolidação das respostas está apresentada na **Seção 7.6 do notebook**.


---

## 7. Autoavaliação

O projeto atingiu os objetivos definidos inicialmente, com a construção de um pipeline de dados completo no Databricks, estruturado nas camadas Bronze, Silver e Gold, utilizando PySpark e tabelas Delta.

O desenvolvimento permitiu realizar desde a ingestão dos dados brutos até sua preparação para consumo analítico. Durante o processo, foram realizados tratamentos e verificações de qualidade, persistência das diferentes camadas no Unity Catalog e análises relacionadas à ocorrência de doença coronariana.

As **cinco perguntas de negócio definidas na Seção 1.3 foram respondidas**, permitindo transformar os dados tratados em informações úteis para análise e concluir os objetivos analíticos estabelecidos para o projeto.

Entre as principais dificuldades encontradas estiveram a organização das etapas do pipeline, a definição dos tratamentos necessários para os dados, a validação das tabelas após a persistência e a estruturação das análises de forma rastreável entre as diferentes camadas.

Como possíveis evoluções do projeto, poderiam ser incorporadas etapas de modelagem preditiva para a variável `TenYearCHD`, avaliação e comparação de modelos e automação da execução do pipeline. Essas extensões permitiriam avançar a partir da análise descritiva realizada neste MVP.

---

## Tecnologias utilizadas

- Databricks
- Apache Spark / PySpark
- Delta Lake
- Unity Catalog
- Python
- GitHub

---

## Arquivos do repositório

- `MVP - Pipeline Framingham.ipynb` — notebook completo desenvolvido e executado no Databricks;
- `framingham.csv.xlsx` — conjunto de dados utilizado no projeto;
- `README.md` — documentação e evidências do projeto.

---

## Como executar

1. Importe o notebook `MVP - Pipeline Framingham.ipynb` para um ambiente Databricks.
2. Disponibilize o conjunto de dados Framingham em um Volume do Unity Catalog.
3. Ajuste, se necessário, o caminho de leitura definido no início do notebook para apontar para o local em que o arquivo foi armazenado.
4. Execute as células do notebook em sequência.

Durante a execução, o notebook realiza a ingestão dos dados, a construção e persistência das camadas Bronze, Silver e Gold, as verificações de qualidade e as análises das perguntas de negócio.

Os nomes de catálogo, schema e caminhos utilizados no projeto estão definidos no próprio notebook e podem ser adaptados de acordo com o ambiente Databricks utilizado.
---

## Autor

Guilherme Montenegro Banharo

Pós-Graduação em Ciência de Dados e Analytics

PUC-Rio
