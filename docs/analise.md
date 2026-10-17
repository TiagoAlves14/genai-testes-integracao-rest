# Comparação crítica dos dois prompts (Unidade III)

## Propósito e cenário comum

Avaliar a geração de testes de integração para a operação de conclusão de sessão do NexoPsi e os efeitos verificáveis sobre o módulo financeiro e PostgreSQL.

| Eixo | Prompt 1: funcional | Prompt 2: estrutural |
| --- | --- | --- |
| Entrada | OpenAPI do cenário | Mesmo OpenAPI + implementação de Scheduling/Billing |
| Foco | Código de status, schemas e valores inválidos/limite | FSM, integração, persistência, auditoria, autorização, isolamento, rollback |
| Casos gerados | 5 (F01–F05) | 7 (E01–E07) |
| Casos executados | 5 testes HTTP | 1 teste HTTP (E05) e 6 testes diretos do handler com banco |
| Aprovação dos casos executados | 5/5 | 7/7 |
| Detecção de defeitos | Nenhum comprovado nesta bateria | Nenhum comprovado nesta bateria |

## Diversidade e qualidade observadas

**Prompt 1:** identificou a validação de formato UUID (422), campo `from` obrigatório, limite `page_size=200`, status HTTP 200 e formato do `SessionOut`. Os cinco casos foram automatizados com FastAPI TestClient e passaram.

**Prompt 2:** identificou caminhos de transição AGENDADA/CONFIRMADA → REALIZADA, rejeição por conclusão repetida (409), sessão inexistente (404), permissão ausente (403), isolamento entre tenants e rollback de transação quando Billing gera uma exceção. Os sete casos foram implementados e passaram.

**Avaliação:** o segundo conjunto amplia as interfaces e estados inspecionados, mas recebeu informação **adicional**. Logo, a comparação é descritiva e **não um experimento que isola apenas o efeito da engenharia de prompts**. Os 12 testes aprovados comprovam comportamento nas condições avaliadas; **não comprovam cobertura de todos os ramos**, ausência de defeitos ou sucesso do pipeline como um todo.

## Ajustes e dificuldades registrados

1. Limitar entradas a contratos e código reais para evitar inferência não sustentada.
2. Exigir rastreabilidade por rota, schema ou módulo.
3. Distinguir expectativas descritas pelo contrato HTTP de códigos de erro evidenciados pelo código.
4. Converter a primeira verificação apenas estática do OpenAPI em cinco chamadas HTTP executadas.
5. Incluir E05 de autorização após observar a ausência do caso na bateria inicial.
6. Corrigir importação e estrutura de `with` no primeiro arquivo de testes para o Ruff; **o lint global do repositório continua falhando**, com 198 apontamentos.
7. Registrar a falha intencional em Billing para E07, comprovando rollback, com ressalva de que o erro foi induzido antes da criação da cobrança.

## Critérios acadêmicos e limitações

O material da Unidade II fundamenta partição de equivalência e valores-limite. A Unidade III (§§3.1.1 e 3.5–3.7) distingue abordagens funcionais e estruturais, recomenda julgamento humano e alerta para testes numerosos, mas inadequados. Não foram medidas cobertura estrutural nem pontuação de mutação; não há base para reportar percentuais.

Detalhes do ambiente, commits, status do lint e matriz de execução: [relatório auditado](relatorio-validacao.md).
