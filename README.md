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

A exploração inicial dos dados e de sua estrutura está documentada nas **Seções 2 e 3 do notebook**, incluindo a visualização inicial do conjunto na **Seção 3.1**.

### Evidência da exploração dos dados

<img width="831" height="290" alt="image" src="https://github.com/user-attachments/assets/4bf0f509-7374-40d0-b6fc-456957a61cfb" />


---

## 2. Carga dos Dados

A ingestão dos dados foi realizada no Databricks utilizando PySpark. O dataset original foi carregado para o ambiente de processamento e utilizado como origem para a construção do pipeline.

A etapa de carregamento e inspeção inicial está documentada na **Seção 3.1 do notebook**.

![Carga dos dados no Databricks](images/02_carga_dados.png)

---

## 3. Modelagem e Catálogo de Dados

O pipeline foi estruturado seguindo a arquitetura Medallion, dividida em três camadas:

- **Bronze:** armazenamento dos dados provenientes da fonte;
- **Silver:** dados tratados, padronizados e validados;
- **Gold:** conjunto final preparado para consumo analítico e modelagem.

As tabelas foram persistidas utilizando Delta e registradas no Unity Catalog do Databricks.

A construção e validação da camada Silver estão documentadas na **Seção 4**, enquanto a construção e validação da camada Gold estão apresentadas na **Seção 5**.

### Camada Silver

A estrutura final da camada Silver pode ser visualizada na **Seção 4.5 do notebook**.

![Camada Silver](images/03_camada_silver.png)

### Camada Gold

A estrutura final da camada Gold pode ser visualizada na **Seção 5.9 do notebook**.

![Camada Gold](images/04_camada_gold.png)

---

## 4. Pipeline de Dados

O pipeline realiza a transformação progressiva dos dados entre as camadas Bronze, Silver e Gold.

Durante a construção da camada Silver foram realizados os tratamentos necessários para padronização e preparação dos dados. Posteriormente, a camada Gold foi construída a partir dos dados tratados e disponibilizada para as análises finais.

A implementação do pipeline está documentada principalmente nas **Seções 4 e 5 do notebook**.

A persistência e validação da camada Silver são apresentadas nas **Seções 4.4 e 4.5**, enquanto a persistência e validação da camada Gold são apresentadas nas **Seções 5.7, 5.8 e 5.9**.

![Pipeline Bronze Silver Gold](images/05_pipeline.png)

---

## 5. Qualidade dos Dados

A qualidade dos dados foi avaliada ao longo do pipeline e consolidada na **Seção 6 do notebook**.

Foram verificadas:

- presença de valores ausentes e registros duplicados (**Seção 6.1**);
- consistência dos valores e domínios das variáveis (**Seção 6.2**);
- presença e análise de valores extremos (**Seção 6.3**);
- síntese final das verificações de qualidade (**Seção 6.4**).

Após os tratamentos realizados, a camada Gold permaneceu com **4.238 registros e 16 atributos**, sem valores ausentes ou registros duplicados.

![Qualidade dos dados](images/06_qualidade_dados.png)

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
