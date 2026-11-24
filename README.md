"""# Detecção de Fraudes em Cartões de Crédito

Este projeto visa criar um modelo de Machine Learning capaz de identificar transações fraudulentas de cartões de crédito utilizando dados históricos.

## 1. O Problema e o Desbalanceamento de Classes
O conjunto de dados apresenta um desbalanceamento extremo de classes, onde apenas **0,17%** das transações são fraudes. Em cenários assim, a métrica de **Acurácia padrão é enganosa**: um modelo ingênuo que classifique tudo como normal obteria 99,83% de acurácia, mas falharia completamente no objetivo de detectar fraudes. 
Por isso, focamos em avaliar o modelo por meio de:
- **Recall (Sensibilidade):** Quantas das fraudes reais conseguimos detectar.
- **Precision (Precisão):** Quantas das transações que alertamos como fraudes realmente eram fraudes.
- **F1-Score:** O equilíbrio harmônico entre precisão e recall.
- **Curva Precision-Recall:** Indicador mais robusto para bases altamente desbalanceadas do que a tradicional curva ROC.

## 2. Preparação de Dados e Engenharia de Recursos (Feature Engineering)
Para enriquecer os dados para o modelo e normalizar escalas discrepantes, foram aplicados os seguintes processos:
- **Logaritmo do Valor:** Aplicação de `log1p` na variável `Amount` para reduzir a assimetria da distribuição de valores.
- **Padronização:** Normalização da variável `Amount` utilizando o `StandardScaler` do Scikit-Learn.
- **Variáveis de Comportamento Temporal (Novas Variáveis):** Para mapear sequências de transações:
  - `Time_Delta`: Tempo decorrido em segundos desde a última transação ocorrida no dataset.
  - `Amount_Rolling_Mean`: Média móvel dos valores das últimas 5 transações.
  - `Amount_Diff_From_Mean`: Desvio do valor da transação atual em relação à sua média móvel recente.

## 3. Comparação entre os Modelos (Métricas para a classe de fraude: Classe 1)

| Modelo | Precision | Recall | F1-Score |
| :--- | :---: | :---: | :---: |
| **Regressão Logística** | 0.87 | 0.66 | 0.75 |
| **Random Forest (Balanced)** | 0.84 | 0.76 | 0.79 |
| **XGBoost (Sem ajuste)** | 0.94 | 0.78 | 0.85 |
| **Regressão Logística (Limiar customizado de 0.3)** | 0.85 | 0.71 | 0.77 |

*Nota: O modelo XGBoost apresentou o melhor equilíbrio geral antes do tuning com GridSearchCV.*

## 4. Limiar de Decisão e Explicabilidade (SHAP)
- **Limiar Customizado (Threshold):** Ao reduzirmos o limiar de decisão de probabilidade da Regressão Logística padrão (de 0.5 para 0.3), aumentamos o **Recall de 0.66 para 0.71**, aceitando uma pequena perda marginal de precisão para garantir que mais fraudes sejam capturadas.
- **Explicabilidade com SHAP:** A análise SHAP revelou que as variáveis mais importantes e com maior impacto absoluto na tomada de decisão do classificador foram a **V4** e a **V14**, seguidas por **V12** e **V10**.

## 5. Estrutura do Repositório
O projeto está estruturado de forma a ler a base diretamente via link público de dados do TensorFlow, evitando o armazenamento e upload de arquivos grandes que superam os limites do GitHub (>100 MB).
"""
