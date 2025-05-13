# 🛍️ Limpeza e Transformação de Dados de Vendas e Locação

## 🔍 **Objetivo**

Este projeto tem como objetivo realizar a limpeza, transformação e agregação de dados de vendas e locação de imóveis extraídos de arquivos JSON, permitindo uma análise clara e precisa dos valores gastos por cada cliente em datas específicas.

---

## 📦 **Instalação de Dependências**

Para garantir que o ambiente de execução possua as bibliotecas necessárias, execute o comando abaixo:

```bash
pip install pandas matplotlib requests
```

---

## 📌 **Importação de Bibliotecas**

As bibliotecas essenciais para a manipulação e análise dos dados são importadas conforme demonstrado a seguir:

```python
import numpy as np
import json
import pandas as pd
import matplotlib.pyplot as plt
import requests
```

---

## 📌 **Leitura dos Dados**

Os dados estão armazenados em arquivos JSON acessíveis através dos links abaixo:

* [Dados de Vendas](https://cdn3.gnarususercontent.com.br/2928-transformacao-manipulacao-dados/dados_vendas_clientes.json)
* [Dados de Locação](https://cdn3.gnarususercontent.com.br/2928-transformacao-manipulacao-dados/dados_locacao_imoveis.json)

A leitura é realizada utilizando a função `requests.get()` para capturar o conteúdo dos links:

```python
# Leitura dos Dados de Vendas
url_vendas = "https://cdn3.gnarususercontent.com.br/2928-transformacao-manipulacao-dados/dados_vendas_clientes.json"
dados_vendas = requests.get(url_vendas).json()

# Leitura dos Dados de Locação
url_locacao = "https://cdn3.gnarususercontent.com.br/2928-transformacao-manipulacao-dados/dados_locacao_imoveis.json"
dados_locacao = requests.get(url_locacao).json()
```

---

## 📌 **Transformação para DataFrame**

Para facilitar a manipulação, os dados são convertidos para DataFrames do Pandas, e, em seguida, normalizados para expandir a estrutura JSON interna:

```python
# Dados de Vendas
dados_vendas = pd.DataFrame(dados_vendas)
dados_vendas = pd.json_normalize(dados_vendas['dados_vendas'])

# Dados de Locação
dados_locacao = pd.DataFrame(dados_locacao)
dados_locacao = pd.json_normalize(dados_locacao['dados_locacao'])
```

---

## 📌 **Renomeando as Colunas**

Para melhor entendimento e padronização, as colunas são renomeadas conforme descrito abaixo:

**Dados de Vendas**

| Coluna Original   | Coluna Renomeada |
| ----------------- | ---------------- |
| data\_da\_venda   | Data da Venda    |
| cliente           | Cliente          |
| valor\_da\_compra | Valor da Compra  |

```python
dados_vendas.rename(columns={
    'data_da_venda': 'Data da Venda', 
    'cliente': 'Cliente', 
    'valor_da_compra': 'Valor da Compra'
}, inplace=True)
```

**Dados de Locação**

| Coluna Original | Coluna Renomeada |
| --------------- | ---------------- |
| data\_locacao   | Data da Locação  |
| locatario       | Locatário        |
| valor\_aluguel  | Valor do Aluguel |

```python
dados_locacao.rename(columns={
    'data_locacao': 'Data da Locação', 
    'locatario': 'Locatário', 
    'valor_aluguel': 'Valor do Aluguel'
}, inplace=True)
```

---

## 📌 **Conversão de Datas para Formato DateTime**

Para facilitar operações futuras, as colunas de data são convertidas para o tipo datetime:

```python
dados_vendas['Data da Venda'] = pd.to_datetime(dados_vendas['Data da Venda'], format='%d/%m/%Y')
dados_locacao['Data da Locação'] = pd.to_datetime(dados_locacao['Data da Locação'], format='%d/%m/%Y')
```

---

## 📌 **Limpeza e Conversão dos Valores Monetários**

Os valores monetários estavam no formato brasileiro (com vírgula e "R\$"). Foi realizada a remoção de símbolos e a conversão para `float`:

```python
# Limpeza em Dados de Vendas
dados_vendas['Valor da Compra'] = dados_vendas['Valor da Compra'].str.replace('R\$', '', regex=True)
dados_vendas['Valor da Compra'] = dados_vendas['Valor da Compra'].str.replace('.', '', regex=False)
dados_vendas['Valor da Compra'] = dados_vendas['Valor da Compra'].str.replace(',', '.', regex=False)
dados_vendas['Valor da Compra'] = dados_vendas['Valor da Compra'].astype('float64')

# Limpeza em Dados de Locação
dados_locacao['Valor do Aluguel'] = dados_locacao['Valor do Aluguel'].str.replace('R\$', '', regex=True)
dados_locacao['Valor do Aluguel'] = dados_locacao['Valor do Aluguel'].str.replace('.', '', regex=False)
dados_locacao['Valor do Aluguel'] = dados_locacao['Valor do Aluguel'].str.replace(',', '.', regex=False)
dados_locacao['Valor do Aluguel'] = dados_locacao['Valor do Aluguel'].astype('float64')
```

---

## 📊 **Visualização Gráfica**

Um gráfico de barras é plotado para demonstrar o total de valores por cliente (vendas) e locatário (locações):

```python
plt.figure(figsize=(12, 6))
plt.bar(dados_vendas['Cliente'], dados_vendas['Valor da Compra'], label='Vendas')
plt.bar(dados_locacao['Locatário'], dados_locacao['Valor do Aluguel'], label='Locação', alpha=0.7)
plt.xlabel('Cliente / Locatário')
plt.ylabel('Valor Total')
plt.title('Total de Compras e Locações')
plt.xticks(rotation=45, ha='right')
plt.legend()
plt.tight_layout()
plt.show()
```

---

## ✅ **Conclusões**

1. Removemos os dados em listas dentro do DataFrame
2. Verificamos os tipos dos dados
3. Realizamos a limpeza e padronização dos dados de vendas e locação.
4. Ajustamos o formato das datas para operações futuras.
5. Convertemos os valores monetários para o formato numérico.
6. Plotamos gráficos para análise clara dos valores totais.

---

## 👨‍💻 **Autor**

**Luiz André de Souza**
GitHub: [brodyandre](https://github.com/brodyandre)

---

## 📄 **Licença**

Este projeto está licenciado sob os termos da MIT License. Consulte o arquivo LICENSE para mais informações.
""
