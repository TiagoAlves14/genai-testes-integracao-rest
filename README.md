# GenAI — Testes de Integração REST

Estudo acadêmico da Unidade III sobre geração e avaliação de testes de integração de APIs REST utilizando IA generativa.

O cenário de referência é o fluxo administrativo de conclusão de sessão e geração de cobrança do sistema NexoPsi. O código-fonte do sistema permanece em repositório privado.

Os cenários, prompts, resultados e limitações serão documentados aqui. Resultados de testes apenas serão divulgados após execução verificável.

## Resultados verificados (17/10/2026)

- **12 casos candidatos** gerados pela LLM: 5 funcionais e 7 estruturais.
- **5 verificações estáticas** do contrato OpenAPI: confirmadas (não são execução HTTP).
- **5 testes novos de integração com PostgreSQL real: 5 aprovados** no GitHub Actions do NexoPsi. Suíte total: **113 aprovados**, 2 warnings, 17,38 s.
- O job de lint da execução consultada falhou; não confundir Pytest aprovado com pipeline completo aprovado.

Consulte o [relatório de execução, limitações e evidências](docs/relatorio-validacao.md). Os testes executáveis ficaram em um PR **privado** do NexoPsi; este repositório público preserva o código original.

[Prompt funcional](prompts/01-funcional.md) · [Prompt estrutural](prompts/02-estrutural.md) · [Casos funcionais](tests/casos-funcionais.json) · [Casos estruturais](tests/casos-estruturais.json) · [Comparação crítica](docs/analise.md) · [Minuta do fórum](forum/postagem.md)
