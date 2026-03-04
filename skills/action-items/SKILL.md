---
name: action-items
description: "Extrai action items de reuniões e cria follow-ups automaticamente (eventos no Calendar, drafts de email, mensagens no Slack). Use sempre que o usuário pedir para revisar notas de reunião, extrair tarefas, criar follow-ups, ou processar transcrições. Trigger quando o usuário mencionar 'action items', 'follow-up', 'follow up', 'tarefas da reunião', 'o que ficou pendente', 'transcrição', 'notas da reunião', 'processar reunião', 'meeting notes'. Também trigger quando o usuário compartilhar uma transcrição ou notas e pedir para organizar os próximos passos."
---

# Action Items — De Reunião para Ação

Você transforma reuniões em ações concretas. Depois de cada reunião, você processa a transcrição ou notas, extrai o que foi decidido e comprometido, e cria automaticamente os artefatos necessários: eventos no Calendar para follow-ups, rascunhos de email para comunicações prometidas, e lembretes via Slack.

## Fontes de transcrição

O usuário pode ter transcrições automáticas geradas por ferramentas como Gemini, Granola, Grain ou Otter.ai, salvas no Gmail e/ou Google Drive. O fluxo de obtenção:

**Opção 1 — Gmail**: busque emails recentes da ferramenta de transcrição ou com assunto contendo nome da reunião.
```
gmail_search_messages com queries como:
- "from:gemini-notes@google.com" (adapte para sua ferramenta)
- "meeting notes" ou "transcrição" ou "meeting summary"
- Filtrar por data (hoje ou data específica)
```

**Opção 2 — Google Drive**: busque documentos de transcrição.
```
google_drive_search com query:
- fullText contains 'meeting' and modifiedTime > '[data]'
- name contains 'transcript' ou 'notes'
```

**Opção 3 — Upload direto**: o usuário pode colar ou fazer upload da transcrição diretamente na conversa.

Se não encontrar transcrição automaticamente, pergunte ao usuário: "Não encontrei a transcrição dessa reunião. Pode colar aqui ou me dizer onde está?"

## Fluxo de trabalho

### 1. Obter e ler a transcrição

Identifique qual reunião processar:
- Se o usuário especificou ("action items da reunião com a Empresa X"), busque essa transcrição específica.
- Se for automático (scheduled task), busque transcrições das reuniões de hoje.
- Use o Google Calendar (`gcal_list_events`) para cruzar: quais reuniões aconteceram hoje e quais transcrições existem.

Leia a transcrição completa. Se estiver no Drive, use `google_drive_fetch`.

### 2. Extrair action items

Ao ler a transcrição, identifique:

**Compromissos explícitos** — alguém disse "eu vou fazer X", "fico de enviar Y", "mando até sexta". Estes são action items claros.

**Decisões tomadas** — "ficou decidido que", "vamos seguir com", "aprovado". Registre como decisões, não como tarefas.

**Pedidos ao usuário** — alguém pediu algo ao usuário: "pode me mandar?", "preciso que você revise". Estes são action items do usuário.

**Pedidos do usuário a outros** — o usuário pediu algo a alguém. Estes viram follow-ups para cobrar.

**Próximos passos mencionados** — "semana que vem a gente revisa", "marcar uma call de follow-up". Estes viram eventos no Calendar.

Para cada item extraído, classifique:
- **Responsável**: quem deve fazer (o usuário ou outra pessoa)
- **Ação**: o que precisa ser feito
- **Prazo**: quando foi mencionado (ou inferir se não explícito)
- **Tipo de artefato**: email, evento no Calendar, mensagem Slack, ou apenas registro

### 3. Entregar action items via Slack DM

<!-- PERSONALIZAR: substitua pelo seu Slack DM channel ID -->
O destino padrão é **Slack DM** (canal `YOUR_SLACK_DM_CHANNEL_ID`). Após extrair os action items, envie uma mensagem consolidada usando `slack_send_message` para esse canal.

Formato da mensagem:

```
:clipboard: ACTION ITEMS — [Nome da Reunião] ([Data])

*Decisões tomadas:*
• [Decisão 1]
• [Decisão 2]

*Seus action items:*
• :red_circle: [Item] — prazo: [data]
• :warning: [Item] — prazo: [data]

*Cobrar de outros:*
• :red_circle: [Pessoa]: [Item] — prazo: [data]
• :warning: [Pessoa]: [Item] — prazo: [data]

*Próxima reunião:* [data/tema, se mencionado]
```

Se o usuário estiver na conversa interativamente, apresente também na conversa do Cowork.

**Nota**: se o usuário pedir explicitamente outro destino (ex: "manda pro Calendar", "cria um draft"), atenda o pedido. Os destinos alternativos disponíveis são:

- **Gmail Draft** (`gmail_create_draft`) — cria rascunho de email pronto para enviar
- **Google Calendar** (`gcal_create_event`) — cria evento de lembrete/follow-up na data do prazo
- **Só no relatório** — sem artefato externo
- **Google Tasks** — não disponível ainda no Cowork

## Quando roda como scheduled task

Ao rodar automaticamente no fim do dia:

1. Consulte o Calendar para listar as reuniões do dia que já aconteceram
2. Para cada reunião, busque a transcrição no Gmail/Drive
3. Processe cada transcrição e extraia os action items
4. Envie a mensagem consolidada via Slack DM (mesmo formato do passo 3 acima)
5. Se houver múltiplas reuniões, agrupe tudo em uma mensagem ou quebre por reunião se ficar longo
6. Se não encontrar transcrições, informe: "Reuniões de hoje sem transcrição disponível: [lista]. Se as transcrições chegarem depois, me peça para processá-las."

## Diretrizes

**Seja específico**: "Mandar proposta para João" é melhor que "Enviar documento". Use nomes, datas e detalhes mencionados na reunião.

**Priorize ações do usuário**: action items do próprio usuário são mais urgentes que itens para cobrar de outros.

**Não invente prazos vagos**: se não mencionaram prazo, registre como "sem prazo definido" e sugira um razoável.

**Emails prontos para enviar**: drafts devem estar completos e revisados — o usuário só precisa clicar enviar. Aplique as diretrizes do `_shared/style-guide.md` e do `humanizer` para tom natural.

**Agrupe por reunião**: quando processar múltiplas reuniões, mantenha os items agrupados por reunião para facilitar a localização.
