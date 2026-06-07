# Exercício 1.2 - Avaliação Manual das Respostas

## Resposta 1

Pergunta: Qual o prazo de devolução?

Resposta do assistente:
"O prazo é de 7 dias úteis, exceto para cargas perigosas classes 1 a 6 da ANTT."

Avaliação: Parcialmente correta

Motivo:
A resposta acerta o prazo de 7 dias úteis, mas não explica corretamente a exceção. Segundo a POL-001, cargas perigosas não podem ser devolvidas pelo processo padrão e devem ser encaminhadas para Gestão de Riscos.

Fonte: POL-001, seções 3.1 e 3.2

---

## Resposta 2

Pergunta: Quanto custa frete para 600kg para Manaus?

Resposta do assistente:
"O frete especial para cargas acima de 500kg para a região Norte tem multiplicador de 1.8 sobre o valor base."

Avaliação: Parcialmente correta

Motivo:
O multiplicador 1.8 está correto para a região Norte na PROC-042-v2. Porém a pergunta era sobre o custo do frete e a resposta não apresenta o valor final nem informa que seria necessário conhecer o valor base para realizar o cálculo.

Fonte: PROC-042-v2, seção 2

---

## Resposta 3

Pergunta: Qual o SLA do cliente Platinum?

Resposta do assistente:
"O cliente Platinum tem resposta em até 1h e resolução em até 12h."

Avaliação: Incorreta

Motivo:
O documento SLA-2024 informa que existem apenas os tiers Gold, Silver e Standard. O tier Platinum não existe, portanto a resposta foi inventada.

Fonte: SLA-2024, seção 1

---

## Resposta 4

Pergunta: Posso devolver carga perigosa?

Resposta do assistente:
"Sim, cargas perigosas podem ser devolvidas em até 7 dias úteis."

Avaliação: Incorreta

Motivo:
A POL-001 informa que cargas perigosas das classes 1 a 6 não são elegíveis para devolução pelo processo padrão. O correto seria orientar o cliente a procurar o setor de Gestão de Riscos.

Fonte: POL-001, seção 3.2

---

## Resposta 5

Pergunta: Qual o multiplicador de frete para o Sudeste?

Resposta do assistente:
"O multiplicador regional para o Sudeste é 1.1."

Avaliação: Correta

Motivo:
A informação está de acordo com a PROC-042-v2, que define o multiplicador 1.1 para a região Sudeste.

Fonte: PROC-042-v2, seção 2.1

---

## Resumo

| Resposta | Resultado |
|-----------|-----------|
| 1 | Parcialmente correta |
| 2 | Parcialmente correta |
| 3 | Incorreta |
| 4 | Incorreta |
| 5 | Correta |

# Rubrica de Avaliação das Respostas

A rubrica abaixo foi criada com apoio do Claude para padronizar a avaliação das respostas geradas pelo assistente da NovaTech.

## 1. Precisão Factual

### Nível 1
A resposta possui informações incorretas ou contradiz a documentação oficial.

### Nível 2
A resposta está majoritariamente correta, mas possui pequenas imprecisões que não comprometem totalmente o entendimento.

### Nível 3
A resposta está totalmente alinhada com a documentação oficial, sem erros ou ambiguidades.

---

## 2. Citação de Fonte

### Nível 1
Não cita fonte ou cita uma fonte incorreta.

### Nível 2
Cita a fonte, mas de forma incompleta ou pouco específica.

### Nível 3
Cita corretamente o documento utilizado, permitindo validação rápida da informação.

---

## 3. Aderência aos Guardrails

### Nível 1
Viola um ou mais guardrails definidos para o projeto.

### Nível 2
Atende parcialmente aos guardrails, mas apresenta pequenas falhas.

### Nível 3
Atende completamente aos guardrails:
- Sempre cita fonte;
- Não inventa valores ou prazos;
- Informa quando não encontra resposta;
- Responde em português formal.

---

## 4. Completude da Resposta

### Nível 1
Resposta incompleta ou superficial.

### Nível 2
Responde à pergunta, mas deixa de apresentar alguma informação relevante.

### Nível 3
Responde completamente à pergunta, incluindo exceções, condições e informações importantes.

---

## Classificação Final

| Pontuação | Resultado |
|------------|------------|
| 11 a 12 | Excelente |
| 8 a 10 | Bom |
| 5 a 7 | Aceitável |
| 4 | Falha |

## Aplicação da Rubrica

| Resposta | Precisão | Fonte | Guardrails | Completude | Total | Resultado |
|-----------|-----------|-----------|-----------|-----------|-----------|-----------|
| 1 | 2 | 3 | 3 | 2 | 10 | Bom |
| 2 | 2 | 3 | 3 | 2 | 10 | Bom |
| 3 | 1 | 1 | 1 | 1 | 4 | Falha |
| 4 | 1 | 1 | 1 | 1 | 4 | Falha |
| 5 | 3 | 3 | 3 | 3 | 12 | Excelente |


## Template de Avaliação Reutilizável

| Pergunta | Resposta | Precisão (1-3) | Fonte (1-3) | Guardrails (1-3) | Completude (1-3) | Total | Observações |
|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|
