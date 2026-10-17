# Relatório de execução e avaliação — NexoPsi / Unidade III

**Data da avaliação:** 2026-10-17  
**Aplicação de origem:** `TiagoAlves14/NexoPsi` (privado)  
**Fluxo:** `POST /sessions/{session_id}/complete` → Agenda → Financeiro → PostgreSQL.  
**Branch de experimento (privada):** `test/genai-integration-unit3`  
**PR privado de testes:** https://github.com/TiagoAlves14/NexoPsi/pull/1  
**Execução do CI:** https://github.com/TiagoAlves14/NexoPsi/actions/runs/38999718073  
**Commit executado:** `0bb4de71c68992b487c191e982f6560eb51626f3`

## Resumo objetivo

- **12 candidatos** produzidos e documentados pela LLM: 5 funcionais (F01–F05) + 7 estruturais (E01–E07).
- **5 verificações estáticas** do contrato OpenAPI real: todas confirmaram a existência das operações, restrições e respostas utilizadas pelos casos F01–F05. **Não são requisições HTTP executadas**.
- **6 testes de integração novos executados:** 6 aprovados, 0 falhas no Pytest.
- **Suíte Pytest completa da execução:** `114 passed, 2 warnings in 15.66s`. Esses 114 incluem os 6 novos testes e 108 testes preexistentes.
- O ambiente foi o GitHub Actions do projeto privado, com `pytest -v`, PostgreSQL por Testcontainers e a role de aplicação com RLS.
- **Job lint:** falhou na execução consultada com 198 achados globais reportados; o arquivo novo não aparece nos apontamentos dessa última execução. A suíte Pytest aprovada não significa pipeline completo aprovado.

## Resultado por cenário — código de integração REAL

| Caso derivado | Procedimento efetivamente executado | Resultado |
|---|---|---|
| E01 | Concluir sessão AGENDADA; consultar sessão, cobrança ABERTA e auditoria no Postgres | **PASSOU** |
| E02 | Concluir sessão CONFIRMADA; consultar sessão, cobrança ABERTA e auditoria no Postgres | **PASSOU** |
| E03 | Repetir conclusão de sessão já REALIZADA; verificar HTTPException 409 e ausência de cobrança duplicada | **PASSOU** |
| E04 | Concluir UUID de sessão inexistente; verificar HTTPException 404 | **PASSOU** |
| E06 | Tenant A tenta concluir sessão de tenant B; validar 404, sessão de B inalterada e nenhuma cobrança | **PASSOU** |
| E07 | Induzir falha no serviço Billing após atualização da sessão e verificar rollback, ausência de cobrança e auditoria parcial | **PASSOU** |

E05 (RBAC sem permissão) **não foi implementado nesta bateria**. O controle RBAC possui testes preexistentes no projeto, mas não se contabiliza isso como execução do novo E05.

Os testes invocam o **handler Python diretamente** e executam as dependências reais de aplicação e persistência; substituem apenas a criação da sessão de banco pelo fixture de PostgreSQL do Testcontainers. Não atravessam um servidor HTTP, gateway ou frontend; por isso, são testes de integração **entre componentes/serviços e banco**, não testes API end-to-end.

## Resultado por cenário — validação estática de contrato

| Caso | Verificação contra OpenAPI | Resultado |
|---|---|---|
| F01 | `POST /sessions/{session_id}/complete` possui resposta 200 e schema `SessionOut` | Confirmado |
| F02 | `session_id` usa formato UUID e resposta 422 está especificada | Confirmado |
| F03 | `SessionOut` declara `status` e `valor_aplicado_centavos` obrigatórios | Confirmado |
| F04 | `GET /financeiro/cobrancas` exige `from` e documenta 422 | Confirmado |
| F05 | `page_size` tem máximo 200 e 422 está documentado | Confirmado |

**Atenção:** nenhuma dessas cinco verificações executou a requisição HTTP correspondente; os casos F01–F05 permanecem `GERADO_NAO_EXECUTADO` na suíte de candidatos, pois somente suas premissas contratuais foram inspecionadas.

## Comparação crítica dos prompts

O prompt funcional derivou entradas inválidas, schemas e respostas a partir de OpenAPI. O estrutural identificou transições e falhas internas no código, alcançando casos que o OpenAPI não documenta explicitamente, como resposta 409 e isolamento 404. Como a **informação disponibilizada à LLM foi diferente**, a comparação não mede isoladamente a superioridade de uma técnica de prompting.

**Ajustes de instrução registrados:** rastreabilidade por arquivo/operação; separação entre documentação e implementação; proibição de resultados sem execução; exigência de fixtures com banco real; distinção entre handler direto, componente mockado e API de ponta a ponta.

## Limitações e próximos passos

1. Implementar testes HTTP com FastAPI TestClient, dados sintéticos e dependências de autenticação, para validar F01–F05 e E05 sem confundir testes de rota com contratos apenas estáticos.
2. O caso E07 foi executado com falha controlada no Billing e assertivas de rollback; manter um teste adicional para falha que ocorre depois da inserção de cobrança, caso necessário.
3. Gerar cobertura de linhas/ramos no Pytest para a suíte delimitada; **nenhuma porcentagem de cobertura foi medida**.
4. Corrigir os 198 apontamentos de lint global em uma atividade separada (fora deste escopo), preservando a evidência de que o Pytest passou.
5. O código de testes executável fica **somente no PR privado**; o repositório público contém os prompts, os cenários, a metodologia e este relatório, sem cópia de código privado.

**Material acadêmico:** UFG/AKCIT, Unidades I, II e III, seções 3.1.1 e 3.5–3.7.
