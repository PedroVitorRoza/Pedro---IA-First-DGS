# Exercício 1.3 — Plano de Testes do Pipeline RAG

## Objetivo

Definir cenários de teste para validar a qualidade do pipeline RAG da NovaTech, cobrindo ingestão de documentos, recuperação de contexto, geração de respostas e aderência aos guardrails.

## Categorias de Teste

### Testes de Ingestão
- Extração de PDFs e DOCX
- Detecção de documentos duplicados
- Validação de encoding
- Rejeição de arquivos inválidos
- Controle de tamanho máximo

### Testes de Retrieval
- Qualidade do chunking
- Geração de embeddings
- Relevância semântica
- Recuperação de versões corretas
- Tratamento de erros de digitação

### Testes de Geração
- Citação correta das fontes
- Recusa quando não há documentação
- Respostas completas
- Tratamento de contradições
- Prevenção de alucinações

### Testes de Contexto
- Lost in the Middle
- Context Overflow
- Persistência em múltiplos turnos
- Priorização da versão mais recente

### Testes End-to-End
- Fluxo completo do sistema
- Múltiplos documentos
- Aplicação dos guardrails
- Latência de resposta

### Testes de Regressão
- Correções permanecem após atualizações
- Guardrails continuam funcionando
- Performance não degrada
