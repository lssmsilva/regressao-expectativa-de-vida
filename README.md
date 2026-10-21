# Agente com RAG (LangChain) sobre um documento, com avaliação crítica

Projeto final de Tópicos Contemporâneos, pós-graduação em Engenharia e Análise de Dados (CESAR School).

Pipeline de **RAG (Retrieval-Augmented Generation)** com **LangChain**: carrega um PDF, divide em chunks, indexa com embeddings no **FAISS** e usa um **agente com tool-calling** (LLM `llama-3.1-8b-instant` via Groq) que decide quando consultar o documento antes de responder. O prompt do agente segue o framework GCIF (Objetivo, Contexto, Instruções, Formato).

## Pipeline

| Etapa | Escolha |
|---|---|
| Documento | Slide de aula "Agents" (21 páginas, ~5.900 caracteres). Não incluído no repositório |
| Splitting | `RecursiveCharacterTextSplitter`, `chunk_size=400`, `overlap=50` (comparado com 200/30 e 600/80) |
| Embeddings | `sentence-transformers/all-MiniLM-L6-v2` (local, gratuito) |
| Vector store | FAISS (28 chunks) |
| Retrieval | `k=2` (testado com 1, 2 e 4) |
| LLM | `llama-3.1-8b-instant` (Groq, tier gratuito) |

## O que a avaliação mostrou (e por que isso é o mais interessante do projeto)

Testei o agente com 6 perguntas e avaliei qualitativamente cada resposta. O resultado foi **misto, e o notebook documenta as falhas em vez de escondê-las**:

- **Retrieval funcionou na maioria dos casos:** em 3 das 4 perguntas de conteúdo, o chunk correto foi recuperado.
- **Geração falhou (falso negativo):** mesmo com o contexto certo, o modelo de 8B respondeu "Não encontrei essa informação" em 3 dessas 4 perguntas.
- **Citação de páginas não confiável:** a ferramenta de busca não repassa o número da página ao LLM, então as páginas citadas em "Fontes" eram inventadas. A correção é passar um `document_prompt` com a página.
- **Falha de retrieval na pergunta sobre os tipos de loop:** a página 13 nunca foi recuperada.
- **Sem alucinação:** o agente nunca inventou fatos externos, e reconheceu corretamente a pergunta fora de escopo e o conteúdo que estava em imagem (limitação do PDF).

## Próximos passos (não implementados)

- Incluir a página no texto devolvido pela ferramenta, para citações reais.
- Testar um LLM maior ou ajustar o prompt para reduzir os falsos negativos.
- Extrair o texto de páginas em imagem (OCR) e revisar o retrieval para tópicos espalhados em várias páginas.
- Avaliar o RAG de forma quantitativa (conjunto de perguntas com respostas esperadas).

## Como executar

1. `pip install -r requirements.txt`
2. Coloque um PDF em `Agents.pdf` (ou altere `PDF_PATH`).
3. Crie uma chave gratuita em <https://console.groq.com/keys>. O notebook pede a chave quando executado (não grave a chave no código).
4. Execute as células em ordem.

## Autora

Laura Silva Soares de Melo · [LinkedIn](https://www.linkedin.com/in/laura-soares-313067195) · [GitHub](https://github.com/lssmsilva)
