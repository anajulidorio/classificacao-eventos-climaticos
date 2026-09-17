# 🌪️ Classificação de Riscos e Estimativa de Eventos Climáticos Extremos

> **Aprendizado Baseado em Problema (ABP) — Machine Learning**  
> *Engenharia de Computação — Centro Universitário SATC (UniSATC)*  
> 🗓️ **Ano:** 2026

---

## 📌 Visão Geral do Projeto

Este projeto utiliza **Algoritmos de Aprendizado Supervisionado (Machine Learning Clássico)** para apoiar o planejamento e a tomada de decisão na gestão de desastres climáticos, atuando como ferramenta de suporte a órgãos como a **Defesa Civil**. 

A partir de dados históricos de desastres naturais no Brasil ocorridos em **2024** (extraídos do **S2iD**), a solução aborda o problema sob duas perspectivas complementares:

1. **🎯 Classificação Multiclasse:** Categorização rápida do **nível de risco/gravidade** do evento (*Baixo*, *Médio* ou *Alto*).
2. **📈 Regressão Quantitativa:** Estimativa contínua do **volume total de população afetada** pelo desastre.

---

## 🎯 Objetivos do Projeto

### 🟢 Objetivo Geral
Desenvolver e comparar modelos de Machine Learning para prever a população afetada e classificar o nível de risco de eventos climáticos extremos no Brasil.

### 🔹 Objetivos Específicos
- 📂 **Coleta e Inspecção:** Estruturar o dataset oficial do S2iD relativo ao ano de 2024.
- 📊 **Análise Exploratória (EDA):** Identificar padrões, qualidade dos dados e distribuições de danos materiais/humanos.
- 🧹 **Pré-Processamento & Feature Engineering:** Limpeza de ruídos, imputação de nulos e criação das variáveis alvo (`Total Afetados` e `Nível de Risco`).
- 🤖 **Treinamento e Otimização:** Testar e comparar múltiplos algoritmos de aprendizado supervisionado.
- 📈 **Avaliação e Importância:** Analisar métricas de desempenho e a relevância das variáveis (*Feature Importance*).

---

## 🤖 Modelos de Machine Learning Utilizados

O projeto avaliou algoritmos supervisionados em duas frentes de modelagem distintas:

### 🎯 1. Modelos de Classificação (Predição do Nível de Risco: Baixo, Médio, Alto)
* **K-Nearest Neighbors (KNN Classifier):** Classificador baseado na proximidade do espaço de atributos das ocorrências.
* **Regressão Logística (Logistic Regression):** Modelo linear baseline parametrizado para multiclasse via *One-vs-Rest* (OvR).
* **Métricas de Avaliação:** *F1-Score (Macro)*, *Precision*, *Recall* e *Matriz de Confusão*. 

### 📈 2. Modelos de Regressão (Estimativa da População Afetada Total)
* **K-Nearest Neighbors (KNN Regressor):** Estimativa contínua pela média ponderada dos vizinhos mais próximos.
* **Regressão Linear Múltipla (Multiple Linear Regression):** Modelo de regressão parametricamente padronizado via `StandardScaler`.
* **Métricas de Avaliação:** *RMSE (Root Mean Squared Error)*, *MAE (Mean Absolute Error)* e *$R^2$ Score (Coeficiente de Determinação)*.

---

## 🗃️ Base de Dados (Dataset)

Os dados utilizados são provenientes do **S2iD (Sistema Integrado de Informações sobre Desastres)**, mantido pelo Ministério da Integração e do Desenvolvimento Regional. 

### 💡 Principais Variáveis do Dominio:
- **Localização:** UF, Município, COBRADE (Classificação de Desastre).
- **Danos Humanos:** Número de mortos, feridos, enfermos, desabrigados, desalojados e desaparecidos.
- **Danos Materiais e Prejuízos (R$):** Impactos na saúde, ensino, habitação, infraestrutura e prejuízos nos setores de agricultura, pecuária, indústria, comércio e serviços.

---

### ⚙️ Tratamentos Especiais Aplicados:

1. **Outliers (Regressão):**
   - **Conceito:** Observações de eventos extremamente catastróficos que distorcem as médias e o aprendizado das equações de regressão (ex: *KNN* e *Regressão Múltipla*).
   - **Solução:** Aplicação do **Intervalo Interquartil (IQR)** na variável `População_Afetada_Total`. Foram filtrados e removidos os registros que ultrapassaram o limite superior tolerado ($Q3 + 3.0 \times IQR$).

2. **Desbalanceamento de Classes (Classificação):**
   - **Conceito:** Ocorre quando eventos de risco *Baixo* são massivamente superiores a eventos extremos de risco *Alto*, podendo induzir o modelo ao vício pela classe majoritária.
   - **Solução:** Utilização de divisão estratificada (`stratify=y_class`) garantindo a mesma proporção de classes nos conjuntos de Treino, Validação e Teste, além da avaliação focada na métrica **F1-Score (Macro)**.

---
## 🏗️ Estrutura do Repositório

```text
Classificacao-eventos-climaticos/
│
├── Datasets/
│   ├── Danos_Informados_Original.xlsx        # Dados brutos extraídos do S2iD
│   └── Danos_Informados.xlsx                 # Base com ruídos controlados para testes de pipeline
│
├── Notebooks/                 
│   ├── Limpeza-e-Pré-processamento.ipynb     # Notebook principal EDA, Pré Processamento, Treinamento.
│   ├── Introdução_de_ruídos.ipynb            # Script para simulação de imperfeições no dataset
│   └── Interface-eventos-climáticos.ipynb    #Script para simulação da interface
│
├── Models/                                   # Modelos exportados após treinamento (.pkl)
│   ├── modelo_1_regressao_knn.pkl            # Modelo KNN treinado para regressão (estimativa da População Afetada Total)
│   ├── modelo_b_classificacao_knn.pkl        # Modelo KNN treinado para classificação do Nível de Risco (Baixo, Médio ou Alto)
│   └── preprocessor.pkl                      # Pipeline de pré-processamento (padronização com StandardScaler e OneHotEncoder)
│
└── README.md                                 # Documentação principal
