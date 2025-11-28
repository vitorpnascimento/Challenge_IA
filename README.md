# Challenge de IA – Predição de Alta Adoção de Telemedicina

Este repositório contém o código e a documentação do **Challenge de Inteligência Artificial / Machine Learning**, em que treinamos um modelo de classificação para identificar **contextos de alta vs baixa adoção de telemedicina (telehealth)** a partir de dados públicos do **CDC (Centers for Disease Control and Prevention)**.

> ⚠️ Importante: o projeto **não** é um serviço de telemedicina, nem faz análise de sintomas ou diagnósticos.  
> É apenas uma **ferramenta analítica/preditiva**, baseada em dados agregados e anonimizados.

---

## 👨‍💻 Integrantes

- **Matheus Farias de Lima – RM554254**
- **Miguel Mauricio Parrado Patarroyo – RM554007**
- **Vitor Pinheiro Nascimento – RM553693**
- **Gabriel Leão – RM552642**

---

## 🎯 Objetivo do Projeto

O objetivo é responder à pergunta:

> “Dado um conjunto de características como grupo demográfico, estado, fase e período da pesquisa,  
> é possível prever se aquele contexto pertence ao grupo de **alta** ou **baixa adoção de telemedicina?**”

Para isso, usamos um dataset público do CDC, criamos uma variável alvo binária (`alta_adocao`) e treinamos um modelo de **Regressão Logística** para fazer a classificação.

---

## 📊 Conjunto de Dados

- **Fonte:** dados públicos do **CDC – Household Pulse Survey (Telemedicine Use)**  
- **Formato:** CSV  
- **Principais colunas usadas:**
  - `Group` – tipo de grupo analisado (ex.: por idade, renda, etc.)
  - `State` – estado ou nível de agregação (“United States”, etc.)
  - `Subgroup` – subgrupo dentro do grupo principal (ex.: faixas etárias)
  - `Phase` – fase da pesquisa
  - `Time Period` – período/semana da pesquisa
  - `Value` – percentual de uso de telemedicina (renomeado para `Pct_Telehealth`)

A partir de `Pct_Telehealth` foi criada a coluna **`alta_adocao`**:

- Calculamos o **percentil 75** de `Pct_Telehealth` (≈ 23,0).  
- `alta_adocao = 1` → `Pct_Telehealth ≥ 23,0` (alta adoção)  
- `alta_adocao = 0` → caso contrário (baixa adoção)

---

## 🧠 Modelagem de Machine Learning

- **Tipo de problema:** Classificação binária (alta vs baixa adoção)
- **Algoritmo:** `LogisticRegression` (scikit-learn)
- **Features (entradas):**
  - `Group`, `State`, `Subgroup`, `Phase`, `Time Period`
- **Pré-processamento:**
  - One-hot encoding nas colunas categóricas (`pd.get_dummies`)
  - Divisão em treino e teste: `70%` treino, `30%` teste (`train_test_split` com `stratify=y`)

---

## 📈 Resultados (Resumo)

No conjunto de teste, o modelo apresentou:

- **Acurácia geral:** ~**0.82** (82%)
- **Classe 0 – baixa adoção**
  - Precision: ~0.85  
  - Recall: ~0.93  
  - F1-score: ~0.89
- **Classe 1 – alta adoção**
  - Precision: ~0.72  
  - Recall: ~0.50  
  - F1-score: ~0.59
- **F1 ponderado:** ~0.81  
- **AUC (Área sob a curva ROC):** ~**0.861**

Também foram geradas:

- **Matriz de confusão** (TN, FP, FN, TP)
- **Curva ROC** com o valor de AUC destacado

Essas imagens estão na pasta `figures/` e são usadas no relatório em PDF.
