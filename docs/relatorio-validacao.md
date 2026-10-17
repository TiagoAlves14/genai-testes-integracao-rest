# Relatório auditado — Unidade III / NexoPsi

**Data da auditoria:** 17/10/2026.  
**Aplicação de origem:** `TiagoAlves14/NexoPsi` (privado), branch `test/genai-integration-unit3`, PR privado [#1](https://github.com/TiagoAlves14/NexoPsi/pull/1).  
**Execução comprovada:** [GitHub Actions #39000265874](https://github.com/TiagoAlves14/NexoPsi/actions/runs/39000265874); commit `aea809ed8a9d1166a28ad6b57eeb74c39252527f`.

## Matriz de atendimento ao enunciado

| Solicitação | Resultado e evidência |
| --- | --- |
| Definir cenário de integração | Conclusão de sessão: `POST /sessions/{session_id}/complete` integra Scheduling, Billing e PostgreSQL |
| Definir dois prompts diferentes | [Prompt funcional/OpenAPI](../prompts/01-funcional.md) e [Prompt estrutural/OpenAPI + código](../prompts/02-estrutural.md) |
| Experimentar com LLM e registrar os casos | GPT-6 gerou [5 casos funcionais](../tests/casos-funcionais.json) e [7 estruturais](../tests/casos-estruturais.json), todos identificados e rastreáveis |
| Registrar os resultados em GitHub | Artefatos publicados neste repositório e testes executáveis preservados no PR privado |
| Comparar diversidade e qualidade | [Análise crítica](analise.md), incluindo limitação de comparabilidade das entradas |
| Registrar ajustes dos prompts | Exigência de rastreabilidade, limites documentados, cenários negativos, dados sintéticos e restrição a resultados comprovados |

## Execução real

**Comando executado pelo job `test` no CI:** `pytest -v`, com ambiente Python 3.12, PostgreSQL 16 via Testcontainers, migrações Alembic e role de aplicação sem bypass de RLS. 

**Resultado real do Pytest:** `120 passed, 2 warnings in 14.15s`. São 12 testes novos + 108 preexistentes; **12/12 novos aprovados**.

| Caso | Escopo executado | Resultado |
| --- | --- | --- |
| F01 | HTTP 200 em conclusão de sessão acessível; dados persistidos | PASSOU |
| F02 | HTTP 422 para UUID inválido no caminho | PASSOU |
| F03 | HTTP 200 e campos obrigatórios do corpo `SessionOut` | PASSOU |
| F04 | HTTP 422 por ausência de `from` em consulta de cobranças | PASSOU |
| F05 | HTTP 422 com `page_size=201` quando o máximo é 200 | PASSOU |
| E01 | Sessão AGENDADA → REALIZADA; criação de cobrança e auditoria no banco | PASSOU |
| E02 | Sessão CONFIRMADA → REALIZADA; criação de cobrança e auditoria | PASSOU |
| E03 | Repetição de conclusão rejeitada (HTTPException 409); sem cobrança duplicada | PASSOU |
| E04 | Sessão inexistente rejeitada (HTTPException 404) | PASSOU |
| E05 | Requisição HTTP sem permissão `session.complete` → 403 e estado inalterado | PASSOU |
| E06 | Tentativa de acesso ao registro de outro tenant → 404; RLS protege registros | PASSOU |
| E07 | Erro induzido no Billing provoca rollback da conclusão, sem cobrança nem auditoria parcial | PASSOU |

Os cinco testes F e E05 usam **FastAPI TestClient** e atravessam as rotas HTTP reais com identidade sintética injetada; não exercitam autenticação real Cognito/JWT nem API Gateway. E01/E02/E03/E04/E06/E07 invocam o handler diretamente, utilizando serviços e PostgreSQL reais, sem servidor HTTP. São **testes de integração**, mas não uma bateria ponta a ponta da aplicação completa.

## Qualidade da execução e limitações

- O job `test` do GitHub Actions foi **aprovado**; o job `openapi` também passou. 
- O job `lint` do mesmo workflow **falhou**, com **198 apontamentos** globais reportados. Portanto, o **workflow geral não passou**. Não afirmar CI completamente verde.
- Foram reportados **dois warnings** na suíte.
- **Não existe medição de cobertura de linhas/branches ou mutação** neste experimento; não afirmar porcentagens.
- O mock utilizado em E07 **injeta uma exceção controlada no ponto de integração com Billing**; sua evidência limita-se à reversão do estado após esse erro, sem testar falha posterior à gravação efetiva da cobrança.
- Os 12 resultados são **apenas da execução que comprovadamente ocorreu**. Nenhum dado pessoal real, segredo, OpenAPI integral ou código-fonte privado foi incluído no repositório público.

## Comparação acadêmica

O Prompt 1 prioriza contrato, parâmetros, validação, equivalência e valores limite; o Prompt 2 prioriza transições, exceções, persistência, autorização, isolamento e atomicidade. A diferença entre resultados reflete **tanto o tipo de prompt quanto o contexto disponível** (o Prompt 2 recebeu código além do OpenAPI), de modo que **não há isolamento causal do efeito do estilo do prompt**. Quantidade de casos e taxa de execução aprovados não são substitutos de cobertura de decisões ou detecção de defeitos. É necessário julgamento humano.

**Ajustes realizados:** rastreabilidade de cada expectativa à especificação/código; proibição de resposta ou comportamento inventado; seleção de cenário implementado; geração de fixtures sintéticas; distinção entre validação estática, execução HTTP e integração direta de componentes; adição de testes HTTP e RBAC após auditoria das lacunas; documentação explícita do lint falho.

**Referências:** Material didático UFG/AKCIT — Unidade II, §2.1 (partição de equivalência e análise de valor limite); Unidade III, §§3.1.1, 3.5–3.7 (prompts funcionais e estruturais, integração REST, revisão e avaliação da qualidade).
