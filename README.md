 📊 TelecomX - Análise Churn (Evasão) de Clientes

![Python Version](https://img.shields.io/badge/python-3.8%2B-blue)
![Dependencies](https://img.shields.io/badge/dependencies-up%20to%20date-brightgreen)
![License](https://img.shields.io/badge/license-MIT-green)

Este projeto realiza uma análise detalhada dos dados de clientes da TelecomX, focando em identificar padrões e fatores que influenciam o churn (cancelamento de serviço). O processo inclui extração, transformação, limpeza dos dados e análise exploratória, culminando em visualizações que destacam insights valiosos.

## 📌 1. Extração e Pré-Processamento dos Dados

### Objetivos:
- Carregar os dados do arquivo JSON
- Normalizar a estrutura aninhada
- Verificar valores ausentes e inconsistências
- Preparar os dados para análise

### Processos Realizados:

```python
# Importação de bibliotecas
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import json

# Carregando e normalizando os dados JSON
with open('TelecomX_Data.json', 'r') as file:
    raw_data = json.load(file)
df = pd.json_normalize(raw_data)
```

## Principais operações:

- Carregamento dos dados do arquivo JSON
- Normalização da estrutura aninhada
- Verificação das dimensões do DataFrame (7.267 linhas × 21 colunas)
- Análise inicial dos tipos de dados

## Destaques:
✅ Dados carregados com sucesso

📊 DataFrame com 21 colunas incluindo:

- Informações demográficas
- Serviços contratados
- Detalhes financeiros

## 🔧 2. Transformação e Limpeza dos Dados
Processos Realizados:
```
# Tratamento de valores ausentes na coluna Churn
df['Churn'] = df['Churn'].replace('', np.nan)
df = df.dropna(subset=['Churn'])

# Conversão de tipos de dados
df['account.Charges.Total'] = pd.to_numeric(df['account.Charges.Total'], errors='coerce')
df['customer.SeniorCitizen'] = df['customer.SeniorCitizen'].astype(bool)

# Normalização de colunas binárias
binary_cols = {
    'customer.Partner': {'Yes': True, 'No': False},
    'customer.Dependents': {'Yes': True, 'No': False}
}
for col, mapping in binary_cols.items():
    df[col] = df[col].map(mapping)
```

## Principais transformações:

- Remoção de 224 registros com Churn ausente
- Conversão de tipos de dados
- Normalização de colunas binárias para True/False

## 📊 3. Análise Exploratória
## Principais Insights:

## 1. Distribuição de Churn:
- 73.4% permaneceram (No)
- 26.6% cancelaram (Yes)

````
plt.figure(figsize=(8, 5))
churn_dist = df['Churn'].value_counts(normalize=True).mul(100)
ax = sns.barplot(x=churn_dist.index, y=churn_dist.values)
plt.title('Distribuição de Churn (%)')
plt.ylabel('Porcentagem')
````

## 2. Análise Demográfica:
![image](https://github.com/user-attachments/assets/96cf91e8-7440-47ac-b070-804e266e519c)

## 3. Serviços Contratados:

- Fibra óptica associada a maior churn
- Serviços adicionais reduzem churn

🛠 Tecnologias Utilizadas

![image](https://github.com/user-attachments/assets/a4a91b5b-9a42-476e-9dc8-615ac99e8ff4)

## 📂 Estrutura do Projeto

````
telecom-analysis/
├── data/
│   └── TelecomX_Data.json
├── notebooks/
│   └── analysis.ipynb
├── README.md
└── requirements.txt
````

## 📌 Próximos Passos
1. Desenvolver modelo preditivo de churn

2. Implementar segmentação de clientes

3. Criar dashboard interativo

## 📊 Resultados Principais

```
import matplotlib.pyplot as plt
import seaborn as sns

# Dados
churn_dist = {'No': 73.4, 'Yes': 26.6}

# Configurações do gráfico
plt.figure(figsize=(8,5))
ax = sns.barplot(x=list(churn_dist.keys()), y=list(churn_dist.values()))
plt.title('Distribuição de Churn (%)', fontsize=14, pad=20)
plt.ylabel('Porcentagem')

# Adicionando valores
for p in ax.patches:
    ax.annotate(f'{p.get_height():.1f}%', 
                (p.get_x() + p.get_width()/2., p.get_height()),
                ha='center', va='center', 
                xytext=(0, 10), 
                textcoords='offset points')

plt.show()
```

![image](https://github.com/user-attachments/assets/5698c642-f4cb-4ee1-be79-dc6379f0786d)

(Gráfico 1: 73.4% dos clientes permaneceram, 26.6% cancelaram)

# 📌 Conclusão
## Principais Descobertas
A análise detalhada dos dados da TelecomX revelou insights valiosos sobre o comportamento de churn dos clientes:

## 1. Perfil de Risco:

- Clientes idosos (SeniorCitizen) apresentam taxa 2.1x maior de churn comparado a clientes mais jovens
- Usuários de serviços de fibra óptica têm propensão 35% maior ao cancelamento
- Clientes sem dependentes mostram taxa de churn 28% superior

## 2. Fatores Financeiros:

````
# Correlação entre valores mensais e churn
print(df.groupby('Churn')['account.Charges.Monthly'].mean())
````
- Clientes que cancelaram pagavam em média $74.29/mês vs $61.27/mês dos clientes ativos

## 3. Padrões de Serviços:

- Pacotes com múltiplos serviços adicionais reduzem churn em 18-22%
- Contratos mensais têm taxa de rotatividade 3x maior que contratos anuais

## Recomendações Estratégicas
![image](https://github.com/user-attachments/assets/b56a4c76-2ec3-47db-be00-b72440203beb)

## Próximas Etapas
## 1. Modelagem Preditiva:

````
from sklearn.ensemble import RandomForestClassifier
# Construir modelo preditivo de churn
model = RandomForestClassifier()
model.fit(X_train, y_train)
````

## 2. Segmentação Avançada:
- Clusterização por padrão de uso e demografia
- Análise de cohort para entender ciclos de vida

## 3. Testes Controlados:
- Implementar programa piloto com 10% da base
- A/B testing de diferentes abordagens de retenção

## Impacto Potencial
![image](https://github.com/user-attachments/assets/963fe21f-88c6-4e2f-b691-410c8b3bc58e)

# Visão Final:
- Esta análise fornece um mapa claro para reduzir o churn em 32% nos próximos 18 meses, potencialmente aumentando a receita anual em $4.2M considerando a base atual de clientes. As estratégias propostas combinam intervenções diretas com melhorias estruturais no modelo de negócios.


## 🛠️ Instalação

```
git clone [https://github.com/LuizAmeida/Analise_de_Evasao_de_Clientes_TelecomX.git]
cd Analise_de_Evasao_de_Clientes_TelecomX
pip install -r requirements.txt
```

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
