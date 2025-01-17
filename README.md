# LegisAssistant: Um sistema de Rag da Legislação Acadêmica da UFAM
Este projeto visa desenvolver uma LLM (Large Language Model) capaz de responder perguntas sobre a legislação acadêmica de Graduação da Universidade Federal do Amazonas (UFAM). Utilizando técnicas avançadas de processamento de linguagem natural e recuperação de informações, o objetivo é criar um chatbot que possa fornecer respostas precisas e contextualmente apropriadas baseadas nas normas e resoluções acadêmicas da UFAM.

Para mais informações sobre as normas acadêmicas da UFAM, acesse [este link](https://proeg.ufam.edu.br/normas-academicas/57-proeg/146-legislacao-e-normas.html).

## Estrutura de arquivos

├── legislacao -> banco de dados vetorial 
│   ├── chroma.sqlite3
│   └── ....
├── Trabalho Final de PLN (1).pdf
├── example.env -> exemplod de .env
├── final.parquet -> arquivo contendo: pergunta | resposta | contexto | resposta_gerada_pelo_modelo
├── indexador.py -> código que cria e indexa arquivos no banco de dador
├── rag_eval.ipynb -> avaliação da qualidade das respostas geradas
└── rag_sys.ipynb -> código fonte do sistema de rag

## Exemplos de Uso (respostas geradas pelo sistema desenvolvido)

- **Pergunta:** Quantas vezes posso faltar em uma disciplina?
- **Resposta:** De acordo com a legislação acadêmica da UFAM, o limite de faltas é de 25% da carga horária total da disciplina.

- **Pergunta:** Quais são os critérios para solicitar a reabertura de matrícula?
- **Resposta:** Os critérios para solicitar a reabertura de matrícula incluem estar dentro do prazo estabelecido no calendário acadêmico e justificar a interrupção dos estudos conforme as normas vigentes.

## Funcionalidades
- Responder perguntas sobre regras de matrícula, prazos, critérios de avaliação e muito mais.
- Recuperar trechos relevantes da legislação para fornecer respostas detalhadas.
- Integrar uma base de dados sintética com exemplos diversos para aprimorar o treinamento do modelo.

## Base de dados
- Para este projeto, utilizei uma base de dados contendo toda legislação da ufam até 18/07/2024
- Essa base contém os textos da legislação, além de perguntas e respostas sobre a mesma
- Para mais informações sobre a base desenvolvida, [consulte aqui](https://huggingface.co/datasets/matiusX/legislacao-ufam)

![Banco de dados](indexacao.png)
Pipeline de indexação dos documentos e criação do banco de dados

## Modelo
- Foi feito o ajuste fino do modelo llama3 utilizando qlora com alpha==16
- Para mais informações do modelo desenvolvido, [consulte aqui](https://huggingface.co/matiusX/lamma-legis-ufam)

## Sistema de rag
- Indexar os documentos da legislação presentes na base de dados utilizando chromaDB
- Pipeline de busca
- - Selecionar pergunta da base de teste -> selecionar chunks -> passar chunks e pergunta no prompt -> comparar com a label de resposta

![Banco de dados](retrieval.png)
Pipeline de retrieval

## Avaliação
| Métrica               | f1 score               | Precision             | Recall                 | Explicação |
|-----------------------|------------------------|-----------------------|------------------------|------------------------|
| **ROUGE-1**           | 0.28797    | 0.25518  | 0.34140     | Mede a sobreposição de unigramas (palavras individuais) entre a resposta gerada e a resposta de referência |
| **ROUGE-2**           | 0.12998    | 0.11259  | 0.16085    | Mede a sobreposição de bigramas (pares de palavras consecutivas) entre a resposta gerada e a resposta de referência |
| **ROUGE-L**           | 0.25525    | 0.22636  | 0.30259    | Mede a sobreposição das subsequências mais longas (LCS - Longest Common Subsequence) entre a resposta gerada e a resposta de referência | 


| Métrica               | valor                | Explicação |
|-----------------------|------------------------|-----------------------|
**BLEU Score** | 0.082019 | BLEU é uma métrica que avalia a qualidade do texto traduzido ou gerado comparando-o com um ou mais textos de referência. A pontuação BLEU considera a precisão dos n-gramas, ajustando para a brevidade, para evitar que frases curtas tenham pontuações artificialmente altas


## Os entregáveis
1. Relatório de Pré-processamento -> pode ser encontrado em [base de dados comentada](https://huggingface.co/datasets/matiusX/legislacao-ufam)
2. Base de Dados Sintética -> pode ser encontrado em [base de dados comentada](https://huggingface.co/datasets/matiusX/legislacao-ufam)
3. Modelo treinado -> [consulte aqui](https://huggingface.co/matiusX/lamma-legis-ufam) (o arquivo "trabalho_final_nlp_treino (1).ipynb" contém o código fonte)
5. Sistema de RAG Implementado -> este repositório

*Os relatórios solicitados são os READMEs dos respectivos repositórios*

## Próximos passos
1. Melhorar avaliação do sistema de RI (gerar os pares (resposta, documento relevante)). Dessa forma, posso avaliar a parte do retrieval de documentos melhor 
2. Avaliação Human-in-the-loop
