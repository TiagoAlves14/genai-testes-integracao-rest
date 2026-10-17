# Prompt funcional (caixa-preta)

Use exclusivamente os trechos de OpenAPI do NexoPsi para as rotas `POST /sessions/{session_id}/complete` e `GET /financeiro/cobrancas`. Gere casos de teste com IDs, entrada, pré-condições, resposta esperada e referência ao contrato. Aplique partições válidas/inválidas e valores-limite apenas quando declarados no contrato. Não invente respostas, dados ou regras; sinalize lacunas. Não afirme execução sem evidência.

**Origem dos parâmetros:** contrato `openapi/nexopsi.json`, commit `6ff82c7` no repositório privado. Caso produzido em experimento conversacional com GPT-6; o arquivo original não é copiado.
