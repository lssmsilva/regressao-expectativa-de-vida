# Predição de expectativa de vida: Regressão Linear vs XGBoost

Projeto da pós-graduação em Engenharia e Análise de Dados (CESAR School).

Prevê a expectativa de vida de países a partir de variáveis socioeconômicas, demográficas e de saúde (dataset *Life Expectancy (WHO)*, 2.938 registros, 2000–2015), compara Regressão Linear e XGBoost e avalia **se o modelo generaliza para países que nunca viu**.

## Resultados

| Avaliação | Regressão Linear | XGBoost |
|---|---|---|
| Teste 30% (split aleatório) — R² | 0,818 | **0,964** |
| Teste 30% — RMSE (anos) | 4,12 | **1,84** |
| Validação cruzada 10 folds — R² médio | 0,776 | **0,913** |
| **Validação por país (países não vistos) — R² médio** | 0,759 | **0,898** |

O XGBoost supera a Regressão Linear em todos os cenários. A queda de R² 0,96 para 0,90 na validação por país mostra o efeito de o mesmo país aparecer no treino e no teste; o número de 0,90 é o mais realista para prever países novos.

## O que o projeto faz

- Análise exploratória: correlações com a variável de saída, redundância entre variáveis (por exemplo `infant deaths` e `under-five deaths`, correlação 0,997).
- Pré-processamento sem vazamento de dados: split antes da imputação, e imputação/padronização ajustadas só no treino (e dentro de cada fold na validação cruzada, via `Pipeline`).
- Métricas: MAE, MAPE, MSE, RMSE e R², com gráficos de real vs predito e de resíduos.
- Validação cruzada (10 folds) e **validação por país com `GroupKFold`**.
- Importância das variáveis do XGBoost.

## Como executar

1. `pip install -r requirements.txt`
2. Coloque o arquivo `Life Expectancy Data.csv` (dataset *Life Expectancy (WHO)*, disponível no Kaggle) na pasta `data/`.
3. Abra `regressao_expectativa_de_vida.ipynb` e execute as células em ordem.

## Autora

Laura Silva Soares de Melo · [LinkedIn](https://www.linkedin.com/in/laura-soares-313067195) · [GitHub](https://github.com/lssmsilva)
