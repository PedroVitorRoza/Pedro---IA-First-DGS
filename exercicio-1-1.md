# Exercício 1.1 — Identificação de Cenários de Falha de IA

## Objetivo

Identificar possíveis cenários de falha para o assistente de IA da NovaTech considerando alucinações, problemas de contexto, recusa inadequada e violações de guardrails.

## Cenários Criados Sem IA
Cenario 1 :
A IA pode alucinar por ter dois arquivos contraditorios
Arquivos esses seriam o PROC-042 e PROC-042-v2
A IA pode inventar uma resposta misturando os dois arquivos
Cenario 2:
O RAG pode usar o FAQ um documento nao oficial para responder as perguntas
Como o documento nao é validado e informal seria um problema a IA responder usando informaçoes pouco confiaveis.
Cenario 3:
A documentaçao nao deixa claro certas regras.
Exemplo: A documentação fala sobre frete acima de 500 kg, se o usuario perguntar "Quanto custa o frete para 300kg" a IA pode responder inventando uma regra para cobrir a falta de informaçoes.
Cenário 4:
A IA pode criar informações para um tier de cliente que não existe.
Exemplo:
O usuário pergunta "Qual o SLA do cliente Platinum?"
A documentação SLA-2024 informa que existem apenas os tiers Gold, Silver e Standard.
Mesmo assim a IA pode inventar tempos de resposta e resolução para um cliente Platinum, gerando uma resposta incorreta.

##Cenarios Gerados pelo Claude
1️⃣ Lost in the Middle
Contexto: Pergunta sobre "Qual é o prazo de entrega padrão?" recupera 5 chunks de documentação, mas a informação correta está no 3º chunk (meio). LLM usa o 1º e 5º (recência/saliência).

⚠️ Risco: Assistente cita chunk desatualizado ou secundário como autoridade, ignorando a versão oficial que existe mas está "perdida" no contexto.

2️⃣ Context Overflow
Contexto: Query: "Como calcular frete?" → RAG retorna 12 chunks (SLA, categorias de produto, tabelas regionais, promoções vigentes). Contexto ultrapassa token limit; LLM trunca silenciosamente.

⚠️ Risco: Informação crítica (ex: tabela de frete para SP) é descartada. Assistente inventa valores ou usa documento anterior de cache. Violação do guardrail #2.

3️⃣ Contradição Documental Não Detectada
Contexto: PROC-042 (2024-01) diz "Frete grátis acima de R$200". PROC-042-v3 (2025-01) diz "Frete grátis acima de R$150". Sistema recupera ambas, mas não sinaliza conflito.

⚠️ Risco: Assistente escolhe uma versão arbitrariamente, sem avisar que há contradição. Cliente recebe informação desatualizada. Falha de guardrail #3 (não dizer explicitamente a dúvida).

4️⃣ Chunk Errado por Semelhança Superficial
Contexto: Pergunta: "Qual taxa aplicada para marketplace internacional?" RAG recupera chunk sobre "Taxa de integração para Anymarket" (produto, não contexto de marketplace externo).

⚠️ Risco: Assistente fornece taxa do produto em vez da política de marketplaces globais. Fonte citada existe, mas é contextualmente errada. Alucinação semântica.

5️⃣ Versionamento Silencioso em URLs/Refs
Contexto: FAQ é hospedado em `docs.novatech.com/faq` (sem versão na URL). Versão 2024 foi atualizada, mas cache do RAG ancora em versão 2023. Sistema cita fonte corretamente, mas dados são defasados.

⚠️ Risco: Guardrail #1 (citar fonte) é atendido, mas guardrail #2 (não inventar) falha sutilmente — os prazos citados são de outro período. Context Rot sem detecção.

6️⃣ Recusa Inadequada por Padrão Conservador
Contexto: Documentação existe sobre "Política de descontos para B2B", mas é vagamente redigida ("aplicar de forma judicialmente apropriada"). Query: "Qual desconto máximo para cliente corporativo?" → Sistema nega resposta por "falta de clareza".

⚠️ Risco: Usuário interno NovaTech fica sem resposta válida e acusa assistente de inútil. Informação existe mas foi recusada. Falha de UX; quebra confiança na ferramenta.

##Lista Final Consolidada

### Cenário 1

**Nome:** Mistura entre PROC-042 e PROC-042-v2

**Categoria:** Informação contraditória

**Origem:** Humano

**Descrição:** A IA pode recuperar as duas versões do procedimento de cálculo de frete especial e misturar valores de multiplicadores regionais ou fatores de peso, gerando uma resposta incorreta.

---

### Cenário 2

**Nome:** Uso do FAQ informal como fonte oficial

**Categoria:** Alucinação

**Origem:** Humano

**Descrição:** O assistente pode utilizar informações do FAQ como se fossem regras oficiais da empresa, sem informar que a fonte é informal e não validada.

---

### Cenário 3

**Nome:** Resposta para frete abaixo de 500kg sem documentação

**Categoria:** Alucinação

**Origem:** Humano

**Descrição:** Como a documentação cobre apenas frete especial acima de 500kg, a IA pode inventar regras ou valores para perguntas sobre cargas menores.

---

### Cenário 4

**Nome:** Criação de SLA para cliente Platinum

**Categoria:** Alucinação

**Origem:** Humano

**Descrição:** A IA pode inventar tempos de resposta e resolução para um tier de cliente inexistente na documentação oficial.

---

### Cenário 5

**Nome:** Lost in the Middle

**Categoria:** Falha de contexto

**Origem:** Claude

**Descrição:** Em perguntas complexas, a informação correta pode estar localizada no meio do contexto recuperado e acabar sendo ignorada pelo modelo.

---

### Cenário 6

**Nome:** Context Overflow

**Categoria:** Falha de contexto

**Origem:** Claude

**Descrição:** Conversas longas ou excesso de chunks recuperados podem ultrapassar a janela de contexto do modelo, causando perda de informações importantes.

---

### Cenário 7

**Nome:** Chunk errado por similaridade

**Categoria:** Falha de contexto

**Origem:** Claude

**Descrição:** O mecanismo de recuperação pode trazer um chunk semanticamente parecido, porém incorreto para a pergunta realizada.

---

### Cenário 8

**Nome:** Recusa inadequada

**Categoria:** Recusa inadequada

**Origem:** Claude

**Descrição:** Mesmo existindo documentação suficiente para responder a pergunta, a IA informa que não encontrou informações.

---

### Cenário 9

**Nome:** Ausência de citação de fonte

**Categoria:** Falha de guardrail

**Origem:** Consolidação

**Descrição:** O assistente responde corretamente, porém não informa qual documento foi utilizado como fonte da informação.

---

### Cenário 10

**Nome:** Resposta em idioma incorreto

**Categoria:** Falha de guardrail

**Origem:** Consolidação

**Descrição:** O assistente responde parcialmente ou totalmente em outro idioma, contrariando o requisito de responder em português formal.


## Casos de Teste dos Cenários Identificados

### Cenário 1 - Mistura entre PROC-042 e PROC-042-v2

**Pergunta de teste:**
Qual o multiplicador regional para o Sudeste?

**Comportamento esperado:**
Responder utilizando a versão mais recente (PROC-042-v2), informando multiplicador 1.1 e citando a fonte.

**Comportamento indesejado:**
Utilizar 1.0, misturar valores das duas versões ou criar um valor intermediário.

**Como verificar:**
Comparar a resposta com a seção 2.1 da PROC-042-v2.

---

### Cenário 2 - Uso do FAQ informal como fonte oficial

**Pergunta de teste:**
Como funciona o seguro de carga?

**Comportamento esperado:**
Informar que não existe documentação oficial sobre o tema na base consultada ou indicar claramente que a informação veio apenas do FAQ.

**Comportamento indesejado:**
Responder como se a informação do FAQ fosse uma regra oficial da empresa.

**Como verificar:**
Validar se a resposta diferencia documento oficial de FAQ informal.

---

### Cenário 3 - Resposta para frete abaixo de 500kg sem documentação

**Pergunta de teste:**
Quanto custa o frete para uma carga de 300kg para Salvador?

**Comportamento esperado:**
Informar que não foi encontrada documentação para fretes abaixo de 500kg.

**Comportamento indesejado:**
Inventar valores, multiplicadores ou regras.

**Como verificar:**
Confirmar que não existe documentação sobre frete padrão na base fornecida.

---

### Cenário 4 - Criação de SLA para cliente Platinum

**Pergunta de teste:**
Qual o SLA do cliente Platinum?

**Comportamento esperado:**
Informar que o tier Platinum não existe e que os únicos tiers são Gold, Silver e Standard.

**Comportamento indesejado:**
Inventar tempos de resposta ou resolução.

**Como verificar:**
Comparar com a seção de classificação de clientes do SLA-2024.

---

### Cenário 5 - Lost in the Middle

**Pergunta de teste:**
Prazo de devolução para carga perigosa e regras de frete especial.

**Comportamento esperado:**
Considerar todos os chunks relevantes, incluindo as exceções da POL-001.

**Comportamento indesejado:**
Ignorar informações localizadas no meio do contexto recuperado.

**Como verificar:**
Validar se todos os chunks esperados do Anexo B foram considerados.

---

### Cenário 6 - Context Overflow

**Pergunta de teste:**
Executar uma conversa longa com múltiplas consultas envolvendo devolução, SLA, frete e atendimento.

**Comportamento esperado:**
Manter consistência nas respostas e preservar informações relevantes.

**Comportamento indesejado:**
Esquecer informações anteriores ou gerar respostas incompletas.

**Como verificar:**
Comparar a qualidade das respostas no início e no final da conversa.

---

### Cenário 7 - Chunk errado por similaridade

**Pergunta de teste:**
Posso devolver carga perigosa?

**Comportamento esperado:**
Utilizar prioritariamente o chunk POL-001-B.

**Comportamento indesejado:**
Basear a resposta apenas no FAQ-03.

**Como verificar:**
Conferir os chunks recuperados pelo sistema.

---

### Cenário 8 - Recusa inadequada

**Pergunta de teste:**
Qual o SLA do cliente Gold?

**Comportamento esperado:**
Responder utilizando a tabela SLA-2024.

**Comportamento indesejado:**
Informar que não encontrou informações suficientes.

**Como verificar:**
Confirmar que a informação existe no documento SLA-2024.

---

### Cenário 9 - Ausência de citação de fonte

**Pergunta de teste:**
Qual o prazo de devolução?

**Comportamento esperado:**
Responder com o prazo e indicar a fonte utilizada.

**Comportamento indesejado:**
Responder sem citar o documento consultado.

**Como verificar:**
Validar a presença da referência POL-001.

---

### Cenário 10 - Resposta em idioma incorreto

**Pergunta de teste:**
Qual o prazo de devolução?

**Comportamento esperado:**
Responder em português formal.

**Comportamento indesejado:**
Responder total ou parcialmente em inglês.

**Como verificar:**
Revisão manual da resposta gerada.

