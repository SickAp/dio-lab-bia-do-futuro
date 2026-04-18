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
transacoes = pd.read_csv('data\transacoes.csv')

#JSONS
with open ('data/perfil_investidor.json', 'r', encoding='utf-8') as f:
    perfil = json.load(f)

with open ('data/produtos_financeiros.json', 'r', encoding='utf-8') as f: 
    produtos = json.load(f)
  
```


### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

Dados injetados no prompt garantindo que o agente tenha um bom contexto. Em soluções mais robustas o dieal é que essas informações sejam careregadas dinamicamente para que possamos ganhar flexibilidade

```text
DADOS DO CLIENTE E PERFIL(data\perfil_investidor.json):

{
  "nome": "João Silva",
  "idade": 32,
  "profissao": "Analista de Sistemas",
  "renda_mensal": 5000.00,
  "perfil_investidor": "moderado",
  "objetivo_principal": "Construir reserva de emergência",
  "patrimonio_total": 15000.00,
  "reserva_emergencia_atual": 10000.00,
  "aceita_risco": false,
  "metas": [
    {
      "meta": "Completar reserva de emergência",
      "valor_necessario": 15000.00,
      "prazo": "2026-06"
    },
    {
      "meta": "Entrada do apartamento",
      "valor_necessario": 50000.00,
      "prazo": "2027-12"
    }
  ]
}



TRANSAÇÕES DO CLIENTE(data\transacoes.csv):

data,descricao,categoria,valor,tipo
2025-10-01,Salário,receita,5000.00,entrada
2025-10-02,Aluguel,moradia,1200.00,saida
2025-10-03,Supermercado,alimentacao,450.00,saida
2025-10-05,Netflix,lazer,55.90,saida
2025-10-07,Farmácia,saude,89.00,saida
2025-10-10,Restaurante,alimentacao,120.00,saida
2025-10-12,Uber,transporte,45.00,saida
2025-10-15,Conta de Luz,moradia,180.00,saida
2025-10-20,Academia,saude,99.00,saida
2025-10-25,Combustível,transporte,250.00,saida

HISTÓRICO DE ATENDIMENTO DO CLIENTE(data\historico_atendimento.csv):

data,canal,tema,resumo,resolvido
2025-09-15,chat,CDB,Cliente perguntou sobre rentabilidade e prazos,sim
2025-09-22,telefone,Problema no app,Erro ao visualizar extrato foi corrigido,sim
2025-10-01,chat,Tesouro Selic,Cliente pediu explicação sobre o funcionamento do Tesouro Direto,sim
2025-10-12,chat,Metas financeiras,Cliente acompanhou o progresso da reserva de emergência,sim
2025-10-25,email,Atualização cadastral,Cliente atualizou e-mail e telefone,sim



PRODUTOS DISPONÍVEIS PARA ENSINO(data\produtos_financeiros.json): 

[
  {
    "nome": "Tesouro Selic",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "100% da Selic",
    "aporte_minimo": 30.00,
    "indicado_para": "Reserva de emergência e iniciantes"
  },
  {
    "nome": "CDB Liquidez Diária",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "102% do CDI",
    "aporte_minimo": 100.00,
    "indicado_para": "Quem busca segurança com rendimento diário"
  },
  {
    "nome": "LCI/LCA",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "95% do CDI",
    "aporte_minimo": 1000.00,
    "indicado_para": "Quem pode esperar 90 dias (isento de IR)"
  },
  {
    "nome": "Fundo Multimercado",
    "categoria": "fundo",
    "risco": "medio",
    "rentabilidade": "CDI + 2%",
    "aporte_minimo": 500.00,
    "indicado_para": "Perfil moderado que busca diversificação"
  },
  {
    "nome": "Fundo de Ações",
    "categoria": "fundo",
    "risco": "alto",
    "rentabilidade": "Variável",
    "aporte_minimo": 100.00,
    "indicado_para": "Perfil arrojado com foco no longo prazo"
  }
]

```

---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

```
DADOS DO CLIENTE:
- Nome: João Silva
- Idade: 32 anos
- Profissão: Analista de Sistemas
- Renda mensal: R$ 5.000,00
- Patrimônio total: R$ 15.000,00
- Perfil de investidor: Moderado
- Aceita risco: Não
- Objetivo principal: Construir reserva de emergência

SITUAÇÃO ATUAL:
- Reserva de emergência atual: R$ 10.000,00
- Valor ideal da reserva: R$ 15.000,00
- Progresso: 66%

METAS FINANCEIRAS:
1. Completar reserva de emergência
   - Valor necessário: R$ 15.000,00
   - Prazo: Junho/2026

2. Entrada de apartamento
   - Valor necessário: R$ 50.000,00
   - Prazo: Dezembro/2027

RESUMO FINANCEIRO (ÚLTIMO MÊS):
- Receita total: R$ 5.000,00
- Despesas totais: R$ 2.488,90

Distribuição de gastos:
- Moradia: R$ 1.380,00
- Alimentação: R$ 570,00
- Transporte: R$ 295,00
- Saúde: R$ 188,00
- Lazer: R$ 55,90

Saldo mensal estimado: R$ 2.511,10

ÚLTIMAS TRANSAÇÕES:
- 01/10: Salário +R$ 5.000,00
- 02/10: Aluguel -R$ 1.200,00
- 03/10: Supermercado -R$ 450,00
- 05/10: Netflix -R$ 55,90
- 07/10: Farmácia -R$ 89,00
- 10/10: Restaurante -R$ 120,00
- 12/10: Uber -R$ 45,00
- 15/10: Conta de Luz -R$ 180,00
- 20/10: Academia -R$ 99,00
- 25/10: Combustível -R$ 250,00

HISTÓRICO DE INTERAÇÕES:
- 15/09: Dúvida sobre CDB (resolvido)
- 22/09: Problema no app (resolvido)
- 01/10: Explicação sobre Tesouro Selic (resolvido)
- 12/10: Acompanhamento de metas financeiras (resolvido)
- 25/10: Atualização cadastral (resolvido)

PRODUTOS DISPONÍVEIS (RESUMO):
- Tesouro Selic: Baixo risco | Indicado para reserva de emergência
- CDB Liquidez Diária: Baixo risco | Rendimento diário
- LCI/LCA: Baixo risco | Isento de IR (carência)
- Fundo Multimercado: Médio risco | Diversificação
- Fundo de Ações: Alto risco | Longo prazo
...
```
