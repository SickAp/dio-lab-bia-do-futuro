# Base de Conhecimento

## Dados Utilizados

Descreva se usou os arquivos da pasta `data`, por exemplo:

| Arquivo | Formato | Utilização no Agente |
|---------|---------|---------------------|
| `historico_atendimento.csv` | CSV | Contextualizar interações anteriores, dar continuidade ao atendimento.|
| `perfil_investidor.json` | JSON | Personalizar recomendações |
| `produtos_financeiros.json` | JSON | Explicar os produtos disponíveis ao cliente |
| `transacoes.csv` | CSV | Analisar padrão de gastos do cliente para uso didático |

> [!TIP]
> **Quer um dataset mais robusto?** Você pode utilizar datasets públicos do [Hugging Face](https://huggingface.co/datasets) relacionados a finanças, desde que sejam adequados ao contexto do desafio.

---

## Adaptações nos Dados

> Você modificou ou expandiu os dados mockados? Descreva aqui.

[Sua descrição aqui]

---

## Estratégia de Integração

### Como os dados são carregados?


Dados injetados diretamente no prompt (CTRL + C, CTRL + V) ou carregar os arquivos via código como no exemplo abaixo:

```python
import pandas as pd
import json

#CSV
historico = pd.read_csv('data\historico_atendimento.csv')
transacoes = pd.read_csv('data\perfil_investidor.json')

#JSONS
with open ('data/perfil_investidor.json', 'r', encoding='utf-8') as f:
    perfil = json.load(f)

with open ('data/produtos_financeiros.json', 'r', encoding='utf-8') as f: 
    produtos = json.load(f)
  
```


### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

```text
DADOS DO CLIENTE:

PERFIL DO CLIENTE:

TRANSAÇÕES DO CLIENTE:

PRODUTOS DISPONÍVEIS PARA ENSINO: 

```

---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

```
Dados do Cliente:
- Nome: João Silva
- Perfil: Moderado
- Saldo disponível: R$ 5.000

Últimas transações:
- 01/11: Supermercado - R$ 450
- 03/11: Streaming - R$ 55
...
```
