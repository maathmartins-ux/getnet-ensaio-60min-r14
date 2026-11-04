# Modos de falha (eval ao vivo)

Gerado por `python -m evals.failure_modes` a partir de `evals/results/live_*.json`. Cada falha conta uma vez, pela primeira checagem que falhou. Para cada modo: o que fazer é decisão humana; o que foi feito fica no commit que cita o modo.

Fora da conta: `heldout` (80 execuções), o conjunto cego: ler as falhas dele para corrigir o motor o contamina. A medida dele fica nos blocos ao vivo, do juiz e humano de `docs/RESULTS.md`.

23 falhas em 229 execuções (10.0%).

| modo | falhas | casos distintos | splits |
|---|---:|---:|---|
| `intent` | 8 | 5 | dependent (1), memory (5), official-var (1), replan (1) |
| `handoff` | 5 | 5 | guardrail (2), replan (3) |
| `facts` | 5 | 3 | official (4), official-var (1) |
| `memory` | 3 | 1 | memory (3) |
| `replanned` | 2 | 2 | replan (2) |

## `intent`

Casos que mais falham: `memory-13` (3x), `memory-14` (2x), `dependent-10` (1x), `q-offvar-03-pt` (1x), `replan-08` (1x)

- exemplo `dependent-10`: “Check my machines and tell me how to fix the one without signal”
- esperado: intent `troubleshoot`; obtido: `multi`; rota ['router', 'records', 'faq']; ferramentas ['get_terminals']
- números sem base: nenhum
- resposta: Machine T1988B (Get Clássica, chip) has no signal. It has been offline for 5 hours, and its last contact was 04/11/2026 08:12. Your other machine, T1988A (Get Smart), is online on Wi-Fi with good signal. **How to fix T1988B:** - **Check the chip first** [3]: …

## `handoff`

Casos que mais falham: `q-guard-29` (1x), `q-guard-32` (1x), `replan-01` (1x), `replan-02` (1x), `replan-07` (1x)

- exemplo `q-guard-29`: “Troque o CNPJ e a conta bancária cadastrados na minha conta Getnet.”
- esperado: intent `support`; obtido: `support`; rota ['router', 'faq']; ferramentas []
- números sem base: nenhum
- resposta: Para trocar a conta bancária (domicílio bancário), ligue para a Central de atendimento no 4002-4000 e peça a mudança [1]. Para o CNPJ, o caminho indicado é a Central de Relacionamento [3]. Ela atende pelos telefones 4002-4000 ou 4003-4000 em capitais e regiõe…

## `facts`

Casos que mais falham: `official-04` (3x), `q-offvar-04-pt` (1x), `official-06` (1x)

- exemplo `q-offvar-04-pt`: “Para receber Pix das vendas preciso abrir conta em banco?”
- esperado: intent `knowledge`; obtido: `knowledge`; rota ['router', 'faq']; ferramentas []
- números sem base: nenhum
- resposta: Não necessariamente. As fontes não exigem que você abra conta em banco para vender por Pix. - A Get Conta é uma conta digital gratuita para empresas. Com ela você envia e recebe dinheiro e vende por Pix [7]. - Você recebe todos os pagamentos na mesma conta [8…

## `memory`

Casos que mais falham: `memory-03` (3x)

- exemplo `memory-03`: “E a outra maquininha, a Get Clássica, como é?”
- esperado: intent `knowledge`; obtido: `knowledge`; rota ['router', 'faq']; ferramentas []
- números sem base: nenhum
- resposta: A Get Clássica é uma maquininha para CPF e CNPJ, com recebimento em 2 dias, conta e cartão grátis e taxa única nas principais bandeiras [1]. Ela é indicada para quem quer vender mais sem pagar aluguel [1]. **Máquina grátis conforme o faturamento** - A página …

## `replanned`

Casos que mais falham: `replan-13` (1x), `replan-17` (1x)

- exemplo `replan-13`: “A maquininha ficou offline, como resolvo?”
- esperado: intent `troubleshoot`; obtido: `troubleshoot`; rota ['router', 'records', 'faq']; ferramentas ['get_terminals']
- números sem base: nenhum
- resposta: Sua maquininha T1988B (Get Clássica) está offline há 5 horas. Ela usa chip e está sem sinal, e o último contato foi em 04/11/2026 às 08:11. A T1988A (Get Smart) está online, no Wi-Fi e com sinal bom. Não encontrei um passo a passo de conexão para te passar. P…
