# Prompt estrutural (caixa-branca)

Considere o contrato OpenAPI, o código de `complete_session` em `scheduling/router.py`, a FSM em `scheduling/service.py`, `on_session_completed` em `billing/service.py`, os repositórios e `tenant_session`/`require_permission`. Gere casos que exercitem decisões, transições, integração Agenda–Financeiro, auditoria, isolamento entre tenants e rollback. Para cada caso, justifique o resultado esperado pelo código e identifique dependências e setup. Não trate mocks como integração completa; não invente taxas de aprovação ou cobertura.

**Referências:** arquivos do projeto privado `TiagoAlves14/NexoPsi`, commit `6ff82c7`. Nenhum trecho privado é reproduzido aqui.
