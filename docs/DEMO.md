# Respostas reais — split `official`

Geradas de `evals/results/live_official.json` (provedor `anthropic`, status `complete`, gasto US$ 0.65); respostas medidas no commit `fdcb685` (2026-11-04T16:15:02Z), página gerada no commit `1a20f8d` em 04/11/2026 16:22 UTC. Nada aqui foi escrito à mão: é a saída do `POST /chat` com o LLM ligado, 1ª rodada de cada caso.

Estabilidade em 3 rodadas: pass@1 0.8667, pass^3 0.8; instáveis: official-06.

Em cada passo, **decidido por** diz quem pôs o agente no turno: o supervisor LLM (tool call); o plano do router (o router LLM, ou a regra que o substitui: seguimento herdado, guarda da própria empresa, queda do LLM), quando o supervisor LLM não foi consultado (rodado pelo atalho de 1 passo ou pelo motor de tabela); o código, com a regra entre parênteses (`supervisor.CODE_WHY`); ou a operadora que respondeu o caso. "não registrado" = passo gravado sem o campo `decided_by`.

| caso | intenção | rota | ferramentas | handoff | veredito automático |
|---|---|---|---|---|---|
| `official-01` | knowledge | router → faq | — | não | OK |
| `official-02` | general | router → general | weather_forecast | não | OK |
| `official-03` | support | router → records | get_settlements | não | OK |
| `official-04` | knowledge | router → faq | — | não | falhou: facts, resolved |
| `official-05` | troubleshoot | router → records → faq | get_terminals | não | OK |
| `official-06` | knowledge | router → faq | — | não | OK |
| `official-07` | general | router → general | currency_rate | não | OK |
| `official-08` | troubleshoot | router → records → faq | get_transactions, get_terminals | não | OK |
| `official-09` | knowledge | router → faq | — | não | OK |
| `official-10` | knowledge | router → faq | — | não | OK |

## official-01

**Pergunta:** What's the difference between the Get Clássica and the Get Smart?

**Intenção:** knowledge (esperada knowledge) · **rota:** router → faq · **replanos:** nenhum · **caminho:** supervisor LLM (laço de tool use) · **custo:** US$ 0.0391 · **latência:** 23.0 s

**Resposta:**

> The Get Clássica is a simpler terminal for individuals or businesses. The Get Smart is a smart Android terminal only for businesses with a CNPJ. [5][6]
>
> **Get Clássica**
> - It's for individuals (CPF) or businesses (CNPJ) who want to sell more without paying rent. [5]
> - It's free with sales from R$10,000 per month. [5]
> - The 2025 page lists these rates: debit 1.69%, credit 1x 3.79%, 2x 7.98%, 3x 8.47%, 4x 8.96% and 5x 9.45%. [5]
> - You receive the money in 2 days. [5]
>
> **Get Smart**
> - It's exclusive to CNPJ with revenue above R$10 thousand per month. It's free with sales from R$60,000. [6]
> - You receive the money in 2 days. [6]
> - On the device itself you can manage stock, take orders, close the bill, load a menu and issue invoices. [1][4]
> - It can replace a computer and a fiscal printer. It also reconciles credit, debit, store credit, QR Code and cash payments. [3]
> - Whether it prints the NF-e depends on the app you choose, though most apps do. [4]
> - App developers charge the app subscriptions. Getnet charges rent and administrative fees. [10]
>
> Both product pages are from 2025 and may be outdated. I couldn't find the Get Smart's debit and credit rates per brand, so I can't compare its rates with the Clássica's. A human agent can help with that if you'd like.

**Passos:**

1. **faq** · partial · motivo `partial_coverage` · 12156 ms · decidido por: supervisor LLM (tool call)

**Fontes:** <https://site.getnet.com.br/duvidas/get-smart/>; <https://site.getnet.com.br/todas-as-maquininhas/>; <https://site.getnet.com.br/duvidas/get-smart/>; <https://site.getnet.com.br/duvidas/get-smart/>; <https://site.getnet.com.br/maquininha/get-classica/>
**Checagens:** 9/9 passam · **desfecho:** parcial (a fonte cobre só parte da pergunta)

## official-02

**Pergunta:** What's the weather forecast in Porto Alegre tomorrow?

**Intenção:** general (esperada general) · **rota:** router → general · **replanos:** nenhum · **caminho:** supervisor LLM (laço de tool use) · **custo:** US$ 0.0138 · **latência:** 7.9 s

**Resposta:**

> Tomorrow (2026-11-05) in Porto Alegre, the forecast is a low of 17 °C and a high of 24 °C. The chance of rain is up to 43%, with drizzle. Source: Open-Meteo [1].

**Passos:**

1. **general** · ok · 4185 ms · decidido por: supervisor LLM (tool call)
   - `weather_forecast` {"city": "Porto Alegre", "day": "amanhã"} → ok

**Fontes:** <https://open-meteo.com/>
**Checagens:** 10/10 passam · **desfecho:** completa

## official-03

**Pergunta:** When will the money from yesterday's sales be deposited?

**Intenção:** support (esperada support) · **rota:** router → records · **replanos:** nenhum · **caminho:** supervisor LLM (laço de tool use) · **custo:** US$ 0.0151 · **latência:** 9.9 s

**Resposta:**

> Yesterday's sales (03/11/2026) are paid out on different dates depending on how the customer paid:
>
> - **Pix:** R$ 120,00, already deposited on 03/11/2026.
> - **Debit:** R$ 95,50, deposited on 04/11/2026.
> - **Credit (30-day plan):** R$ 480,00, scheduled for 03/12/2026.
>
> The total is R$ 695,50 across 3 sales. Of that, R$ 215,50 has already been deposited and R$ 480,00 is still to come.

**Passos:**

1. **records** · ok · 3393 ms · decidido por: supervisor LLM (tool call)
   - `get_settlements` {"period": "yesterday"} → ok · `{"ok": true, "data": {"period": "yesterday", "from": "2026-11-03", "to": "2026-11-03", "today": "2026-11-04", "schedule": [{"method": "pix"…`

**Checagens:** 10/10 passam · **desfecho:** completa

## official-04

**Pergunta:** Do I need a bank account to receive my sales via Pix?

**Intenção:** knowledge (esperada knowledge) · **rota:** router → faq · **replanos:** nenhum · **caminho:** supervisor LLM (laço de tool use) · **custo:** US$ 0.0244 · **latência:** 13.5 s

**Resposta:**

> It depends. The sources don't say outright that you need a bank account to receive Pix sales, but they do show where the money goes.
>
> - Pix is available on Getnet's physical terminals, on TEF Auttar and in e-commerce [5].
> - Get Conta is a free digital account for your business. With it you can send and receive money and sell through Pix [7][9].
> - Receiving sales is one of the transactions that keeps a Get Conta active [6].
> - The Pix receipt time can vary by up to one calendar day. It depends on the Pix model you're eligible for and on when the sale is approved [8].
>
> The sources don't say whether you can receive Pix sales in an account at another bank instead of a Get Conta. I can pass your question to a human agent if you'd like.

**Passos:**

1. **faq** · partial · motivo `partial_coverage` · 6678 ms · decidido por: supervisor LLM (tool call)

**Fontes:** <https://site.getnet.com.br/duvidas/>; <https://site.getnet.com.br/duvidas/pix-mais-respostas/>; <https://site.getnet.com.br/pix/>; <https://site.getnet.com.br/duvidas/pix-mais-respostas/>; <https://site.getnet.com.br/duvidas/>
**Checagens:** 7/9 passam; falham: facts, resolved · **desfecho:** não resolvida

## official-05

**Pergunta:** My card machine won't connect to the internet, what should I do?

**Intenção:** troubleshoot (esperada troubleshoot) · **rota:** router → records → faq · **replanos:** ['records:need->faq (maquininha offline: se a pergunta é sobre conexão ou sinal, o passo a passo vem )'] · **caminho:** supervisor LLM (laço de tool use) · **custo:** US$ 0.0382 · **latência:** 26.9 s

**Resposta:**

> Your offline machine is T1988B (Get Clássica). It's on chip with no signal, and it last made contact at 08:08 on 04/11/2026. Your other machine, T1988A (Get Smart), is online.
>
> To get T1988B connected, try these steps [1][6]:
> 1. With the machine turned off, open the cover on the back.
> 2. Put the chip in the slot marked SIM1 or SIM2, with the metal part facing down.
> 3. Close the cover, turn the machine on, and wait for it to connect to the data network.
> 4. When it shows a 2G or 3G network, it's ready to use.
> 5. Run a communication test. Press the function button (F, # or the one on your machine), choose option 2 (maintenance), then option 1 (communication test).
>
> The sources don't say what to do if there's still no signal after the test.
>
> I can propose a support ticket for T1988B with the reason "no signal" or "no connection". Nothing runs until you confirm, and the proposal expires after 15 minutes. Do you want me to propose it?

**Passos:**

1. **records** · ok · 4068 ms · decidido por: supervisor LLM (tool call)
   - `get_terminals` {} → ok · `{"ok": true, "data": {"terminals": [{"terminal_id": "T1988A", "model": "Get Smart", "status": "online", "connection": "wi-fi", "signal": "b…`
2. **faq** · partial · motivo `partial_coverage` · 13075 ms · decidido por: supervisor LLM (tool call)

**Fontes:** <https://site.getnet.com.br/duvidas/>; <https://site.getnet.com.br/todas-as-maquininhas/>; <https://site.getnet.com.br/crediario/>; <https://site.getnet.com.br/todas-as-maquininhas/>; <https://site.getnet.com.br/duvidas/pix/>
**Checagens:** 10/10 passam · **desfecho:** parcial (a fonte cobre só parte da pergunta)

## official-06

**Pergunta:** How does receivables advance (antecipação) work with Getnet?

**Intenção:** knowledge (esperada knowledge) · **rota:** router → faq · **replanos:** nenhum · **caminho:** supervisor LLM (laço de tool use) · **custo:** US$ 0.0288 · **latência:** 21.0 s

**Resposta:**

> Antecipação lets you receive credit-card sales, paid in full or in installments, before the normal settlement date [6].
>
> - **How to request it:** Use the Getnet Brasil app, which has no extra fees to use [9].
> - **Timing:** A schedule can be advanced from D+1 (one business day after the sale) until D-3 (three business days before it settles) [8].
> - **Hours:** Santander account holders can check or request an advance Monday to Friday, 9am to 9pm, except holidays. Other banks have different hours [7].
> - **Limits:** Some card brands can't be advanced through Getnet. They can only be advanced through the brand's own operator [8].
> - **Automatic advance:** This is a condition for the exemption on the "Receba Conforme suas Vendas" plan. It goes through credit approval, and Getnet assesses each contract individually [2].
> - **Loans with collateral (gravame):** Your sales from Getnet and other acquirers are paid into your Get Conta, up to the loan's outstanding balance [1].
>
> I didn't find the fee for a standard manual advance. A human agent can confirm it if you'd like.
>
> I can also propose a credit receivables advance for an amount you choose. You would have to confirm it, and then a human agent would have to approve it before it runs.

**Passos:**

1. **faq** · partial · motivo `partial_coverage` · 11746 ms · decidido por: supervisor LLM (tool call)

**Fontes:** <https://site.getnet.com.br/duvidas/antecipacao-de-vendas/>; <https://site.getnet.com.br/duvidas/>; <https://site.getnet.com.br/duvidas/antecipacao-de-vendas/>; <https://site.getnet.com.br/duvidas/antecipacao-de-vendas/>; <https://site.getnet.com.br/duvidas/link-de-pagamento/>
**Checagens:** 9/9 passam · **desfecho:** parcial (a fonte cobre só parte da pergunta)

## official-07

**Pergunta:** What's the euro exchange rate today?

**Intenção:** general (esperada general) · **rota:** router → general · **replanos:** nenhum · **caminho:** supervisor LLM (laço de tool use) · **custo:** US$ 0.0144 · **latência:** 7.3 s

**Resposta:**

> Today's euro rate (PTAX closing bulletin, 2026-11-04) is R$ 6.1531 to buy and R$ 6.1549 to sell. Rounded, that is R$ 6.15 for both.
>
> Source: Banco Central do Brasil, https://www.bcb.gov.br/estabilidadefinanceira/historicocotacoes

**Passos:**

1. **general** · ok · 2802 ms · decidido por: supervisor LLM (tool call)
   - `currency_rate` {"currency": "EUR", "date": "2026-11-04"} → ok

**Fontes:** <https://www.bcb.gov.br/estabilidadefinanceira/historicocotacoes>
**Checagens:** 10/10 passam · **desfecho:** completa

## official-08

**Pergunta:** My card machine is showing a transaction decline error.

**Intenção:** troubleshoot (esperada troubleshoot) · **rota:** router → records → faq · **replanos:** ['records:need->faq (venda recusada: o que fazer vem do manual de recusas; maquininha offline: se a p)'] · **caminho:** supervisor LLM (laço de tool use) · **custo:** US$ 0.0383 · **latência:** 26.2 s

**Resposta:**

> The data doesn't show a terminal fault on T1988A.
>
> Two sales were declined today, both on T1988A:
> - **TX1007** (10:05, credit, R$ 210,00): code 51 means insufficient balance or credit limit. The buyer should contact the card issuer or pay with another card or Pix.
> - **TX1008** (10:40, debit, R$ 75,00): code 05 means not authorized by the issuer. The buyer should contact their bank. Retrying will not help.
>
> The two declines total R$ 285,00. In the same period you had 6 approved sales, totaling R$ 995,40.
>
> T1988A is online on Wi-Fi with good signal. Your other terminal, T1988B, has been offline for 5 hours with no signal. None of these declines came from it.
>
> I can propose a support ticket for T1988B (no signal), or for a transaction error. It only runs after you confirm. I can also hand you to a human agent.

**Passos:**

1. **records** · ok · 4874 ms · decidido por: supervisor LLM (tool call)
   - `get_transactions` {"period": "last_30_days"} → ok · `{"ok": true, "data": {"period": "last_30_days", "from": "2026-10-06", "to": "2026-11-04", "today": "2026-11-04", "sales": [{"transaction_id…`
   - `get_terminals` {} → ok · `{"ok": true, "data": {"terminals": [{"terminal_id": "T1988A", "model": "Get Smart", "status": "online", "connection": "wi-fi", "signal": "b…`
2. **faq** · partial · motivo `partial_coverage` · 12030 ms · decidido por: supervisor LLM (tool call)

**Fontes:** <https://site.getnet.com.br/duvidas/>; <https://site.getnet.com.br/duvidas/operacional/>; <https://site.getnet.com.br/todas-as-maquininhas/>; <https://site.getnet.com.br/duvidas/pix-mais-respostas/>; <https://site.getnet.com.br/get-ajuda-erros-maquininha/erro-adq-1-87/>
**Checagens:** 10/10 passam · **desfecho:** parcial (a fonte cobre só parte da pergunta)

## official-09

**Pergunta:** How many installments can I split a sale into with the crediário?

**Intenção:** knowledge (esperada knowledge) · **rota:** router → faq · **replanos:** nenhum · **caminho:** supervisor LLM (laço de tool use) · **custo:** US$ 0.0158 · **latência:** 12.5 s

**Resposta:**

> You can split a sale into up to 48 installments with the Crediário, on a credit card [1][3]. The exact maximum depends on the card issuer. Each issuer sets its own installment limit, and the maximum term and interest rate vary by issuer [2][4].
>
> To see the interest rate and final amount, run a simulation on the card machine. Insert the card, enter the amount, then choose credit > crediário. The simulation is for reference only, and the issuer approves the transaction [2].

**Passos:**

1. **faq** · ok · 6523 ms · decidido por: supervisor LLM (tool call)

**Fontes:** <https://site.getnet.com.br/crediario/>; <https://site.getnet.com.br/duvidas/crediario/>; <https://site.getnet.com.br/crediario/>; <https://site.getnet.com.br/crediario/>; <https://site.getnet.com.br/duvidas/parcelado/>
**Checagens:** 9/9 passam · **desfecho:** completa

## official-10

**Pergunta:** Can I sell through WhatsApp using the Payment Link?

**Intenção:** knowledge (esperada knowledge) · **rota:** router → faq · **replanos:** nenhum · **caminho:** supervisor LLM (laço de tool use) · **custo:** US$ 0.0144 · **latência:** 9.1 s

**Resposta:**

> Yes, you can sell through WhatsApp with the Payment Link. You send the link to your customers by WhatsApp, social networks, email or SMS. They click it, choose the payment method and complete the purchase [1]. Getnet's Payment Link page also lists selling through WhatsApp or a social network as a benefit [2].

**Passos:**

1. **faq** · ok · 4267 ms · decidido por: supervisor LLM (tool call)

**Fontes:** <https://site.getnet.com.br/duvidas/link-de-pagamento/>; <https://site.getnet.com.br/link-de-pagamento/>; <https://site.getnet.com.br/link-de-pagamento/>; <https://site.getnet.com.br/duvidas/link-de-pagamento/>; <https://site.getnet.com.br/duvidas/link-de-pagamento/>
**Checagens:** 9/9 passam · **desfecho:** completa
