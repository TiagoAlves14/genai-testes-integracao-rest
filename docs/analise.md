# Análise crítica — geração de casos

A geração pela LLM resultou em **5 casos funcionais** (F01–F05) e **7 estruturais** (E01–E07). Eles constam dos JSONs com status `GERADO_NAO_EXECUTADO`.

O prompt funcional delimitou casos a parâmetros, respostas e schemas do OpenAPI (200/422, UUID, limites de paginação). O estrutural favoreceu cenários intermodulares (criação de cobrança, transição 409, RBAC 403, isolamento 404, rollback). **Não se pode inferir qualidade maior somente pela contagem**: o segundo prompt recebeu código que o primeiro não recebeu.

## Ajustes e revisão

Os prompts foram ajustados para (1) exigir referência verificável; (2) distinguir respostas documentadas de comportamento apenas encontrado na implementação; (3) não atribuir sucesso nem cobertura sem execução; (4) não considerar mocks prova de integração; (5) restringir a dados sintéticos. São ajustes editoriais documentados; não há benchmark medido de versões antes/depois.

## Estado da validação

Os cenários ainda não haviam sido executados contra a aplicação na geração inicial. A validação posterior está registrada separadamente em `docs/relatorio-validacao.md` quando disponível. Sem Docker/Testcontainers ou ambiente equivalente não se pode confirmar as integrações Postgres/RLS.

## Referência acadêmica

UFG/AKCIT, Unidade III, seções 3.5–3.7: integração de APIs REST, qualidade, dependência do prompt e necessidade de inspeção humana. Unidade II: partição de equivalência e análise de valores-limite.
