# Código da Aplicação

Esta pasta contém o código do seu agente financeiro.

## Estrutura Sugerida

```
src/
├── app.py              # Aplicação principal (Streamlit/Gradio)
├── agente.py           # Lógica do agente
├── config.py           # Configurações (API keys, etc.)
└── requirements.txt    # Dependências
```

## Exemplo de requirements.txt

```
streamlit
openai
python-dotenv
```

## Como Rodar

```bash
# Instalar dependências
pip install -r requirements.txt

# Rodar a aplicação
streamlit run app.py
```

## Código pronto
```python


import json
import pandas as pd
import requests
import streamlit as st

#==========CARREGAR DADOS==============

OLLAMA_URL = "https//localhost:numero_da_porta/api/generate" # substituir numero_da_porta pela porta onde o Ollama está rodando.

MODELO = "gpt-oss" 

perfil = json.load(open('./data/perfil_investidor.json'))
transacoes = pd.read_csv('./data/transacoes.csv')
historico = pd.read_csv('./data/historico_atendimento.csv')
produtos = json.load(open('./data/produtos_financeiros.json'))


#============MONTAR CONTEXTO=================

contexto = f"""
CLIENTE: {perfil['nome']}, {perfil['idade']} anos, perfil {perfil['perfil_investidor']}
OBJETIVO: {perfil['objetivo_principal']}
PATRIMÔNIO: R$ {perfil['patrimonio_total']} | RESERVA: R$ {perfil['reserva_emergencia_atual']}

TRANSAÇÕES RECENTES:
{transacoes.to_string(index=False)}

ATENDIMENTOS ANTERIORES:
{historico.to_string(index=False)}

PRODUTOS DISPONÍVEIS:
{json.dumps(produtos, indent=2, ensure_ascii=False)}
"""

# ========= SYSTEM PROMPT ==========
SYSTEM_PROMPT = """Você é o Alex, um agente de educação financeira que realiza consultorias financeiras. 


OBJETIVO:
Seu objetivo é educar financeiramente os clientes explicando conceitos e estratégias de forma didática e acessível.

REGRAS:
- NUNCA recomende investimentos específicos, apenas explique como funcionam;
- JAMAIS responda a perguntas fora do tema ensino de finanças pessoais.
  Quando ocorrer, responda lembrando do seu papel de educador financeiro;
- Use os dados fornecidos para dar exemplos personalizados;
- Linguagem simples, como se explicasse para um amigo;
- Se não souber algo, admita: "Não tenho essa informação, mas posso explicar...";
- Sempre pergunte se o cliente entendeu;
- Ofereça outras formas de didática como exemplos visuais(mapas mentais, ilustrações, uso de tópicos, detalhamento) caso o cliente não tenha entendido;
- Responda de forma sucinta e direta, com no máximo 3 parágrafos.
"""

# ========= CHAMAR OLLAMA ==========
def perguntar(msg):
    prompt = f"""
{SYSTEM_PROMPT}

CONTEXTO DO CLIENTE:
{contexto}

Pergunta: {msg}"""

    r = requests.post(OLLAMA_URL, json={"model": MODELO, "prompt": prompt, "stream": False})
    return r.json()['response']

# ========= INTERFACE ==========
st.title("Alex, Seu Consultor Financeiro")

if pergunta := st.chat_input("Sua dúvida sobre finanças..."):
    st.chat_message("user").write(pergunta)

    with st.spinner("..."):
        st.chat_message("assistant").write(perguntar(pergunta))

        #rodar o código - instalar as dependências necessárias (pip install) e iniciar o Ollama localmente para testar a aplicação.


```
