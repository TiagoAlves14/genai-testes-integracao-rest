# Postagem para o fórum — Unidade III

Para o experimento, escolhi o **NexoPsi**, uma aplicação web administrativa com API REST em FastAPI. O cenário foi a integração entre **Agenda e Financeiro**: ao concluir uma sessão por `POST /sessions/{session_id}/complete`, a aplicação altera o estado da sessão e gera uma cobrança no banco PostgreSQL, com auditoria.

Utilizei **dois prompts diferentes** com GPT-6. O primeiro, **funcional (caixa-preta)**, utilizou o contrato OpenAPI para propor testes de status HTTP, formato UUID, campos da resposta, parâmetros obrigatórios e limites de paginação. Ele gerou **cinco casos**. O segundo, **estrutural (caixa-branca)**, utilizou o OpenAPI e o código dos componentes para explorar transições de sessão, criação de cobrança, rejeição por estado inválido, controle de permissão, isolamento entre tenants e rollback. Ele gerou **sete casos**.

Os doze casos foram implementados como testes automatizados. No GitHub Actions do NexoPsi, **todos os 12 testes novos passaram**: cinco casos funcionais por HTTP, seis casos estruturais atravessando os componentes e PostgreSQL, além de um caso estrutural de autorização por HTTP. A suíte Pytest completa registrou **120 testes aprovados, dois avisos e duração de 14,15 segundos**. O job de lint geral apresentou **198 apontamentos**, por isso não considero o pipeline inteiro aprovado.

A principal diferença foi a **diversidade dos comportamentos explorados**. O primeiro prompt produziu testes mais próximos do contrato e suas validações; o segundo alcançou caminhos de execução e efeitos transacionais que não estavam detalhados no OpenAPI. No entanto, o segundo também recebeu **mais informações** (código-fonte). Portanto, não é possível concluir que apenas o tipo de prompt causou essa diferença.

Durante a atividade, ajustei as instruções para exigir rastreabilidade das expectativas, impedir a invenção de campos ou regras, diferenciar código de documentação e obrigar evidências antes de afirmar aprovação. Também ampliei a validação: os primeiros casos funcionais haviam sido apenas conferidos estaticamente e depois foram executados via HTTP; o caso de autorização foi acrescentado e executado.

Os testes foram executados com **dados sintéticos**. A autenticação HTTP utilizou identidade simulada, e os testes estruturais utilizaram PostgreSQL real via Testcontainers. Não medi percentual de cobertura de código nem executei o frontend/gateway de ponta a ponta.

Repositório público dos artefatos: https://github.com/TiagoAlves14/genai-testes-integracao-rest

**Referências:** UFG/AKCIT, Unidade II (partição de equivalência e análise de valor-limite) e Unidade III (§§3.1.1 e 3.5–3.7, técnicas de prompt e testes de integração REST).
