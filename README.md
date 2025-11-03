Com certeza. Aqui está um `README.md` completo para o seu projeto, com o destaque obrigatório da Licença MIT no topo e no final, conforme solicitado.

Você pode copiar e colar este conteúdo diretamente em um arquivo chamado `README.md`.

-----

# 🤖 Agente Autônomo de Análise Fiscal (v1.0)

[](https://opensource.org/licenses/MIT)

**O conteúdo deste repositório e o código-fonte estão licenciados sob a [Licença MIT](#6-️-licença).**

## 1\. 📖 Visão Geral

Este projeto é um protótipo avançado de um **Agente Autônomo de IA** projetado para automatizar a análise complexa de documentos fiscais. Executado inteiramente no ambiente Google Colab, ele fornece uma interface de usuário (frontend) amigável com Gradio, que permite a qualquer usuário fazer upload de um arquivo `.zip` contendo CSVs e realizar consultas sofisticadas em linguagem natural.

O núcleo da solução é um **Agente LangChain (Pandas Agent)** que vai além de simples "Perguntas e Respostas". Ele ativamente classifica, categoriza e modifica os dados com base em regras de negócio dinâmicas, que são injetadas de acordo com o **ramo de atividade** selecionado pelo usuário (Indústria, Agronegócio, etc.).

## 2\. ✨ Features Principais

  * **Interface Web no Colab:** UI interativa e de fácil uso construída com Gradio para upload de arquivos e chat.
  * **Agente Especialista (Context-Aware):** O agente carrega "regras de negócio" (prompts) diferentes com base no ramo de atividade selecionado (Indústria, Agronegócio, Setor Automotivo, etc.).
  * **Classificação Automática:** O agente é instruído a sempre tentar classificar os dados (ex: `Tipo_Documento`, `Centro_Custo`) como tarefa primária, enriquecendo a análise.
  * **Alta Disponibilidade (Fallback):** Tenta executar a análise com **OpenAI (GPT-4/4o)** e, em caso de falha (cota, erro de API), aciona automaticamente o **Google Gemini 1.5/2.5 Pro** como fallback.
  * **Segurança de Chaves:** As chaves de API (`OPENAI_API_KEY`, `GOOGLE_API_KEY`) são gerenciadas de forma segura usando o **Cofre de Senhas (Secrets)** do Colab, nunca sendo expostas no código.
  * **Leitura Robusta de CSV:** Lida com diferentes separadores (`,` e `;`) e ignora linhas mal formatadas (`on_bad_lines='skip'`), evitando falhas comuns de leitura.
  * **Respostas Formatadas:** Instruído a gerar saídas complexas (listas, totais, comparações) em tabelas Markdown para melhor legibilidade.

## 3\. 🛠️ Stack Tecnológica

  * **Ambiente de Execução:** Google Colab
  * **Frontend (UI):** Gradio
  * **Backend / Agente:** LangChain (`create_pandas_dataframe_agent`)
  * **Modelos de Linguagem (LLMs):** OpenAI (Primário), Google Gemini 1.5/2.5 Pro (Fallback)
  * **Gerenciamento de Chaves:** Google Colab Secrets
  * **Bibliotecas Principais:** Pandas, Langchain-Experimental

## 4\. 🚀 Guia de Execução Rápida

Para executar este projeto, abra o Google Colab e siga os 5 passos abaixo **em ordem**.

### Passo 1: Configurar o Cofre de Senhas (Secrets)

Antes de tudo, armazene suas chaves de API com segurança:

1.  Na barra lateral esquerda do Colab, clique no ícone de **chave (🔑)**.
2.  Adicione o secret `OPENAI_API_KEY` com sua chave da OpenAI.
3.  Adicione o secret `GOOGLE_API_KEY` com sua chave do Google (Gemini).
4.  **Importante:** Ative o "Acesso ao notebook" (toggle switch) para ambas as chaves.

### Passo 2: Executar Célula 1 (Instalações)

Esta célula instala todas as dependências (`gradio`, `langchain`, `openai`, `langchain-google-genai`, etc.).

```python
# CÉLULA 1: Instalações
print("Instalando dependências...")
!pip install gradio pandas langchain langchain-experimental langchain-community ...
```

### Passo 3: Executar Célula 2 (Carregar Chaves)

Esta célula lê as chaves do Cofre de Senhas de forma segura e as carrega nas variáveis de ambiente.

```python
# CÉLULA 2: Leitura Segura das Chaves de API
import os
from google.colab import userdata
os.environ["OPENAI_API_KEY"] = userdata.get('OPENAI_API_KEY')
os.environ["GOOGLE_API_KEY"] = userdata.get('GOOGLE_API_KEY')
...
```

### Passo 4: Executar Célula 3 (Criar o Backend)

Esta célula usa o comando mágico `%%writefile` para criar o script `agente_csv.py` no disco do Colab. Este arquivo contém toda a lógica do agente.

```python
# CÉLULA 3: Criação do Arquivo do Agente (Backend)
%%writefile agente_csv.py
import os
import pandas as pd
from langchain_google_genai import ChatGoogleGenerativeAI
from langchain_experimental.agents import create_pandas_dataframe_agent
...
```

### Passo 5: Executar Célula 4 (Lançar o Frontend)

Esta célula final importa o agente (`agente_csv`) e lança a interface Gradio, que ficará acessível através de um link público.

```python
# CÉLULA 4: Importação e Lançamento do Frontend
import gradio as gr
import agente_csv 
...
demo.launch(share=True, debug=True)
```

Após executar a Célula 4, um link público (`...gradio.live`) aparecerá na saída. Clique nele para abrir a interface do agente em seu navegador.

## 5\. 🔧 Como Customizar (Manutenção)

A principal vantagem deste agente é a facilidade de manutenção. A "inteligência" de negócios não está no código, mas no **prompt de texto**.

Para adicionar um novo ramo de atividade (ex: "Saúde"):

1.  **Edite a Célula 4:** Adicione `"Saúde"` à lista `ramos_de_atividade`.
2.  **Edite a Célula 3:** Na função `get_prompt_especializado`, adicione um novo bloco `elif`:
    ```python
    elif ramo_atividade == "Saúde":
         prompt_especialista += "REGRAS DE SAÚDE: Monitore códigos TUSS, repasses de convênio, etc.\n"
    ```
3.  Re-execute as Células 3 e 4. O agente agora está "treinado" para o ramo de Saúde.

## 6\. ⚖️ Licença

**O conteúdo deste projeto está licenciado sob a Licença MIT.**

Copyright (c) 2025 [Seu Nome ou Nome da Organização Aqui]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
