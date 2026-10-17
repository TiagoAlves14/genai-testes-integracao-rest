# Fórum — Unidade III (minuta, completar após execução)

**Cenário:** NexoPsi, conclusão de sessão pela rota `POST /sessions/{session_id}/complete`, integrando Agenda e Financeiro, com persistência e auditoria.

**Prompts:** (1) funcional, baseado apenas em OpenAPI, com foco em parâmetros, validação e contratos; (2) estrutural, baseado em OpenAPI + código, com foco em decisões, transições de estado e persistência intermodular.

**Experimento:** GPT-6 gerou inicialmente 5 casos funcionais e 7 estruturais. O primeiro enfatizou `200/422` e parâmetros declarados; o segundo incluiu hipóteses de `403/404/409`, transações e criação de cobrança, fundamentadas no código de referência. A diferença também reflete o **contexto adicional**, não apenas a instrução.

**Ajustes:** inclusão obrigatória de rastreabilidade, distinção entre contrato e implementação e proibição de resultados inventados. 

**Evidência:** casos JSON e relatório de validação. Os casos gerados não comprovam, por si, execução; não afirmar cobertura ou aprovação sem os registros reais.

**Repositório:** https://github.com/TiagoAlves14/genai-testes-integracao-rest

**Referências:** Unidades I, II e III do material de testes de software com GenAI (UFG/AKCIT).
