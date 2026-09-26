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

## 6. Análise de Dados

A análise final está apresentada na **Seção 7 do notebook** e responde às cinco perguntas de negócio definidas no início do projeto.

As análises estão organizadas da seguinte forma:

- **Seção 7.1:** ocorrência de doença coronariana por faixa etária;
- **Seção 7.2:** ocorrência de doença coronariana e hipertensão;
- **Seção 7.3:** indicadores clínicos e ocorrência de doença coronariana;
- **Seção 7.4:** tabagismo e ocorrência de doença coronariana;
- **Seção 7.5:** diabetes e ocorrência de doença coronariana;
- **Seção 7.6:** síntese das respostas às perguntas de negócio.

As cinco perguntas de negócio propostas foram respondidas utilizando os dados tratados e disponibilizados na camada Gold.

### Evidência das análises

![Respostas às perguntas de negócio](images/07_analise_dados.png)

---

## 7. Autoavaliação

O projeto atingiu o objetivo de construir um pipeline de dados completo no Databricks, estruturado nas camadas Bronze, Silver e Gold, utilizando PySpark e tabelas Delta.

O desenvolvimento permitiu realizar desde a ingestão dos dados brutos até sua preparação para consumo analítico. Durante o processo foram realizadas verificações de qualidade, persistência das tabelas e análises relacionadas à ocorrência de doença coronariana.

As **cinco perguntas de negócio definidas na Seção 1.3 foram respondidas**, permitindo transformar os dados tratados em informações úteis para análise.

Entre os principais desafios estiveram a organização das etapas do pipeline, o tratamento e validação dos dados e a estruturação das análises de forma rastreável entre as diferentes camadas.

Como possíveis evoluções do projeto, poderiam ser incorporadas etapas de modelagem preditiva, avaliação de modelos e automação da execução do pipeline.

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
