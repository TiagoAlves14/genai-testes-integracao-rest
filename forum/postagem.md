# Fórum — Unidade III (minuta, completar após execução)

**Cenário:** NexoPsi, conclusão de sessão pela rota `POST /sessions/{session_id}/complete`, integrando Agenda e Financeiro, com persistência e auditoria.

**Prompts:** (1) funcional, baseado apenas em OpenAPI, com foco em parâmetros, validação e contratos; (2) estrutural, baseado em OpenAPI + código, com foco em decisões, transições de estado e persistência intermodular.

**Experimento:** GPT-6 gerou inicialmente 5 casos funcionais e 7 estruturais. O primeiro enfatizou `200/422` e parâmetros declarados; o segundo incluiu hipóteses de `403/404/409`, transações e criação de cobrança, fundamentadas no código de referência. A diferença também reflete o **contexto adicional**, não apenas a instrução.

**Ajustes:** inclusão obrigatória de rastreabilidade, distinção entre contrato e implementação e proibição de resultados inventados. 

**Evidência de execução:** seis testes estruturais novos (E01, E02, E03, E04, E06 e E07) passaram com PostgreSQL/Testcontainers no GitHub Actions do NexoPsi; a suíte completa registrou 114 aprovados, 2 avisos e 15,66 s. Os cinco casos funcionais foram verificados estaticamente no contrato, mas ainda não executados por HTTP. O job de lint geral falhou com 198 apontamentos; não houve medição de cobertura de código. Relatório: https://github.com/TiagoAlves14/genai-testes-integracao-rest/blob/main/docs/relatorio-validacao.md .

**Repositório:** https://github.com/TiagoAlves14/genai-testes-integracao-rest

**Referências:** Unidades I, II e III do material de testes de software com GenAI (UFG/AKCIT).
