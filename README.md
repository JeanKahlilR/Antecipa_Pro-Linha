# Antecipa Pro-Linha 

[![Python](https://img.shields.io/badge/Python-3.9+-blue.svg)](https://www.python.org/)
[![CRISP-DM](https://img.shields.io/badge/Methodology-CRISP--DM-orange.svg)](https://pt.wikipedia.org/wiki/CRISP-DM)
[![Status](https://img.shields.io/badge/Status-Conclu%C3%ADdo-green.svg)]()

O **Antecipa Pro-Linha** é um pipeline de ponta a ponta de Ciência de Dados voltado para a **Indústria**. O objetivo principal é prever falhas mecânicas em equipamentos de um parque fabril monitorados por sensores, evitando paradas inesperadas na linha de produção e combatendo o efeito *Garbage In, Garbage Out*.


## 📑 Metodologia CRISP-DM

Este projeto foi estruturado seguindo o framework **CRISP-DM** (Cross Industry Standard Process for Data Mining), garantindo uma abordagem rigorosa e alinhada com o mercado.


```text
+------------------------+      +------------------------+
|  1. Entendimento do    | ---> |   2. Entendimento dos  |
|       Negócio          |      |          Dados         |
+------------------------+      +------------------------+
                                          |
                                          v
+------------------------+      +------------------------+
|     4. Modelagem       | <--- |   3. Preparação dos    |
|                        |      |          Dados         |
+------------------------+      +------------------------+
|
v
+------------------------+
|     5. Avaliação       |
+------------------------+
```

### 1. Entendimento do Negócio
No cenário da Indústria Moderna, paradas não planejadas geram prejuízos massivos. Atuamos com uma abordagem de **manutenção preditiva** para classificar se uma máquina vai falhar (`Falha = 1`) ou operar normalmente (`Falha = 0`), permitindo intervenções cirúrgicas e antecipadas da equipe de engenharia.

### 2. Entendimento dos Dados
A base de dados conta originalmente com 10.000 registros e 14 variáveis capturadas por sensores.

#### 📝 Dicionário de Dados
*   `udi`: Identificador único numérico (1 a 10.000).
*   `id_produto`: Código exclusivo do produto (Qualidade + Número de série).
*   `tipo`: Classe do equipamento baseado na especificação: **L** (Low), **M** (Medium) ou **H** (High).
*   `temperatura_ar_k`: Temperatura ambiente em Kelvin.
*   `temperatura_processo_k`: Temperatura gerada no processo em Kelvin.
*   `velocidade_rotacao_rpm`: Velocidade de giro do motor.
*   `torque_nm`: Força de torção gerada pelo motor.
*   `desgaste_ferramenta_min`: Tempo acumulado de uso da ferramenta em minutos.
*   **`falha_maquina` (Variável Alvo)**: `1` para falha mecânica, `0` para normal.

> ⚠️ **Nota Técnica:** As colunas `falha_twf`, `falha_hdf`, `falha_pwf`, `falha_osf` e `falha_rnf` detalham o motivo técnico pós-quebra. Elas servem apenas para histórico e **foram excluídas do treinamento do modelo** para evitar vazamento de dados (*data leakage*).

### 3. Preparação dos Dados (Data Prep)
Adotamos o conceito de **Arquitetura Medalhão** para garantir a linhagem dos dados (*data lineage*):
*   **Camada Bronze:** Dados brutos originais do sensores salvos localmente.
*   **Camada Silver:** Dados limpos e sanitizados. 
    *   *Tratamento de Nulos:* Foi identificada a ausência de 500 registros simultâneos em 4 variáveis preditoras (5% da base). Como apenas 3% destes casos continham falhas, optou-se pela exclusão dos registros para evitar a introdução de vieses.
*   **Camada Gold / Engenharia de Recursos:** Criação de variáveis estratégicas, normalização de escala com `StandardScaler` e balanceamento de classe utilizando a técnica estatística **SMOTE** (Synthetic Minority Over-sampling Technique) para mitigar o desbalanceamento severo (apenas 3.3% de falhas).

### 4. Modelagem
Para resolver o problema de classificação binária e controlar o risco de *overfitting* (sobreajuste), foram testados e comparados algoritmos de Machine Learning com ajuste refinado de hiperparâmetros:
*   K-Neighbors Classifier (`KNeighborsClassifier`)
*   Decision Tree Classifier (`DecisionTreeClassifier`)

### 5. Avaliação
A seleção do modelo final baseou-se nas métrica de **Acurácia**



## 📁 Estrutura do Repositório

```text
Antecipa_Pro-Linha/
├── data/                                          # Armazenamento de Dados (Ignorado no Git)
│   ├── 1_bronze/                                  # Dados brutos originais
│   ├── 2_silver/                                  # Dados limpos e tratados
│   └── 3_gold/                                    # Tabelas prontas para modelagem
├── docs/                                          # Escopo e vídeo de apresentação do projeto
├── notebooks/                                     # Onde o pipeline completo foi desenvolvido
│   ├── sanitização_Raw-Silver.ipynb               # ETL e Limpeza dos dados
│   └── Analise_Exploratória_Silver-Gold.ipynb     # Analise exploratória dos dados e Feature engineering
│   └── Pipeline_Completo_Pro-Linha.ipynb          # Pipeline completo indo do ETL até o modelo preditivo
├── outputs/                                       # Subprodutos do código
│   └── graficos/                                  # Gráficos exportados automaticamente
├── .gitignore                                     # Proteção de arquivos locais/pesados
├── README.md                                      # Apresentação do projeto
└── requirements.txt                               # Dependências externas do projeto
```
## 🛠️ Como Executar o Projeto

Você pode executar este projeto utilizando o **Anaconda / Jupyter Notebook** ou o **VS Code**. Siga as instruções abaixo:

### 📋 Pré-requisitos
Certifique-se de ter o Python instalado (recomendado versão 3.9 ou superior) ou o gerenciador de pacotes Anaconda.

### Passo 1: Clonar o Repositório

Baixe o repósitorio

### Passo 2: Instalar as Dependências
Instale as bibliotecas externas necessárias listadas no arquivo requirements.txt:

```bash
pip install -r requirements.txt
```
### Passo 3: Executar via Jupyter Notebook (Opção A)
1. Abra a pasta onde foi baixado o repositorio com o terminal e digite:
```bash
jupyter notebook
```
2. Abra o arquivo:
```text
Pipeline_Completo_Pro-Linha.ipynb
```
3. No menu superior, clique em Cell > Run All para rodar o pipeline completo.

### Passo 4: Executar via VS Code (Opção B)

1. Abra a pasta do projeto no VS Code:
```bash
code .
```

2. Certifique-se de ter a extensão oficial Jupyter da Microsoft instalada.

3. Abra os notebook `Pipeline_Completo_Pro-Linha.ipynb` dentro da pasta notebooks/.

4. Selecione o Kernel do seu Python (ou ambiente Conda) no canto superior direito e clique no botão Run All (Executar Tudo).
