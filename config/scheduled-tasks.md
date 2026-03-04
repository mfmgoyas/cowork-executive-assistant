# Scheduled Tasks — Configuração de Crons

As skills abaixo podem rodar automaticamente via scheduled tasks do Claude Cowork. Para configurar, peça ao Claude: "Cria uma scheduled task para [skill]" ou use os exemplos abaixo.

## executive-digest — Resumo matinal

**Quando**: toda manhã, seg-sex, 30 minutos antes do início do expediente.

**Cron sugerido**: `0 8 * * 1-5` (8:00 AM, seg-sex)

**Prompt para criar a task**:

```
Gere meu Executive Briefing seguindo TODAS as diretrizes da skill executive-digest.
Verifique o dia da semana para determinar se é briefing diário (seg-qui) ou semanal (sexta).
Colete TODOS os dados antes de compilar: Gmail (unread + transcrições de reunião), Calendar (amanhã),
Slack (canais diários + radar pares/diretos + menções). Compile e envie via Slack DM
para o canal YOUR_SLACK_DM_CHANNEL_ID. Não pare no meio — a tarefa só termina quando a mensagem
chegar no Slack.
```

---

## meeting-prep — Preparação de reuniões

**Quando**: toda manhã, seg-sex, logo após o digest.

**Cron sugerido**: `30 8 * * 1-5` (8:30 AM, seg-sex)

**Prompt para criar a task**:

```
Prepare briefings para todas as minhas reuniões de hoje. Para cada reunião no meu
Google Calendar que tenha participantes externos, pesquise: histórico de emails no Gmail,
conversas recentes no Slack, documentos compartilhados no Drive, e informações públicas
sobre participantes externos. Monte um briefing escaneável para cada reunião e me envie
via Slack DM para o canal YOUR_SLACK_DM_CHANNEL_ID.
Use o formato e diretrizes da skill meeting-prep.
```

---

## action-items — Follow-up de reuniões

**Quando**: toda tarde/noite, seg-sex, após o horário típico da última reunião.

**Cron sugerido**: `0 18 * * 1-5` (18:00, seg-sex)

**Prompt para criar a task**:

```
Revise as reuniões que tive hoje consultando meu Google Calendar. Para cada reunião,
busque a transcrição no Gmail ou Google Drive (geradas pelo Gemini ou outra ferramenta).
Extraia action items, decisões e compromissos. Envie a lista consolidada via Slack DM
para o canal YOUR_SLACK_DM_CHANNEL_ID.
Use o formato e diretrizes da skill action-items.
```

---

## Como criar as tasks no Cowork

Abra o Claude Cowork e diga:

```
Cria uma scheduled task chamada "executive-digest" que roda todo dia de semana
às 8h da manhã com este prompt: [cole o prompt acima]
```

Ou use a ferramenta diretamente se preferir controle total:

```
Nome: executive-digest
Cron: 0 8 * * 1-5
Prompt: [prompt acima]
```

## Personalização

Ajuste os horários conforme seu fuso horário e rotina:

- **Digest**: 30 min antes de você começar a trabalhar
- **Meeting prep**: logo após o digest, para ter tempo de ler antes da primeira reunião
- **Action items**: 30 min após seu horário típico de última reunião

Os crons usam o fuso horário local da sua máquina (não UTC).

**IMPORTANTE**: substitua `YOUR_SLACK_DM_CHANNEL_ID` pelo ID real do seu canal de DM. Para encontrar: abra seu DM consigo mesmo no Slack, clique no nome do canal no topo, e copie o Channel ID (começa com `D`).
