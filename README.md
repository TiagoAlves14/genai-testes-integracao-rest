# GenAI — Testes de Integração REST | Unidade III

Experimento acadêmico de geração, execução e avaliação de testes de integração com IA generativa, fundamentado no **NexoPsi**, aplicação administrativa com backend Python/FastAPI e PostgreSQL. O repositório de origem é privado; **nenhum código-fonte privado nem dado real de paciente foi divulgado aqui**.

## Resultado validado em 17/10/2026

- **Dois prompts com enfoques diferentes:** funcional a partir do OpenAPI e estrutural a partir do OpenAPI e código-fonte.
- **12 casos gerados por GPT-6:** cinco funcionais e sete estruturais.
- **12 testes novos executados e aprovados**: seis HTTP com FastAPI TestClient (F01–F05 e E05) e seis de integração entre serviços e banco (E01–E04, E06 e E07).
- **Pytest completo:** `120 passed, 2 warnings in 14.15s` (12 novos + 108 preexistentes).
- **CI completo não aprovado:** job lint falhou com 198 apontamentos globais. Não houve medição de cobertura percentual.

## Evidências e artefatos

- [Relatório técnico auditado](docs/relatorio-validacao.md)
- [Análise comparativa dos prompts](docs/analise.md)
- [Prompt 1 — funcional](prompts/01-funcional.md)
- [Prompt 2 — estrutural](prompts/02-estrutural.md)
- [Casos funcionais](tests/casos-funcionais.json)
- [Casos estruturais](tests/casos-estruturais.json)
- [Postagem preparada para o fórum](forum/postagem.md)
- [Execução comprovada do GitHub Actions](https://github.com/TiagoAlves14/NexoPsi/actions/runs/39000265874) — requer acesso autorizado ao NexoPsi
- [PR privado com código executável](https://github.com/TiagoAlves14/NexoPsi/pull/1) — não integrado à main

**Nota metodológica:** os testes HTTP injetam uma identidade sintética na dependência de autenticação FastAPI; os testes de componentes usam PostgreSQL real via Testcontainers. Não foram testados Cognito, API Gateway ou frontend de ponta a ponta. Os dois prompts variam também pela quantidade de código fornecido à LLM; os resultados não permitem atribuir diferenças unicamente à formulação do prompt.

**Referências:** material didático UFG/AKCIT, Unidades I, II e III, com foco nos critérios funcionais e em testes de integração REST apoiados por GenAI.
