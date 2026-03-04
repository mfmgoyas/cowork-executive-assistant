# Customization Guide — Como Personalizar Cada Skill

Este guia detalha o que personalizar em cada skill para que o assistente funcione como **seu** EA pessoal.

## Visão geral

| Arquivo | O que mudar | Prioridade |
|---|---|---|
| `_shared/style-guide.md` | Seu perfil de escrita | Alta |
| `executive-digest/SKILL.md` | Slack IDs, canais, pares, contexto org | Alta |
| `action-items/SKILL.md` | Slack DM channel ID | Média |
| `config/scheduled-tasks.md` | Horários e channel IDs | Média |
| `email-drafting/SKILL.md` | Nenhuma (usa style-guide) | — |
| `meeting-prep/SKILL.md` | Nenhuma | — |
| `humanizer/SKILL.md` | Nenhuma (usa style-guide) | — |

Procure os comentários `<!-- PERSONALIZAR -->` nos arquivos para encontrar rapidamente os pontos de personalização.

---

## 1. Style Guide (`_shared/style-guide.md`)

Este é o arquivo mais importante. Ele define como o Claude escreve na sua voz — usado por `humanizer`, `email-drafting` e `action-items`.

### O que preencher

**Identidade**: seu nome, cargo, empresa, idiomas e fuso horário.

**Regra de idioma**: quando usar cada idioma baseado no destinatário. Por exemplo, equipe interna em PT-BR, parceiros internacionais em inglês.

**Estilo Slack**: como você abre mensagens (saudação, direto ao ponto), tom (direto, humorado, assertivo), padrões de linguagem (uso de @mentions, emojis, colchetes), e fechamento.

**Estilo Email**: mesmo formato, mas para emails. Separe por idioma se necessário.

**Anti-padrões**: frases que você NUNCA diria. Exemplos: "Certamente!", "Fico feliz em ajudar", "Conforme mencionado anteriormente". Inclua a alternativa que você usaria.

**Contextos especiais**: como você lida com feedback difícil, coordenação de grupo, comunicação com stakeholders externos.

### Atalho para preencher

Peça ao Claude:

```
Analisa meus últimos 20 emails enviados e 50 mensagens no Slack,
e preenche o style-guide pra mim.
```

O Claude vai buscar padrões reais da sua comunicação e sugerir o conteúdo de cada seção. Depois é só revisar e ajustar.

---

## 2. Executive Digest (`executive-digest/SKILL.md`)

Esta é a skill mais personalizada. Sem customização, o digest não sabe o que monitorar.

### Dados do usuário

Encontre a seção de dados do usuário e preencha:

- **Slack user ID**: seu ID de usuário no Slack (formato `U0XXXXXXXXX`). Para encontrar: clique no seu perfil no Slack → "..." → "Copy member ID".
- **DM channel ID**: o ID do canal de DM consigo mesmo (formato `D0XXXXXXXXX`). Para encontrar: abra seu DM consigo mesmo → clique no nome do canal no topo → copie o Channel ID.
- **Email**: seu email corporativo.
- **Timezone**: seu fuso horário (ex: `America/Sao_Paulo`).

### Canais diários

Liste os canais Slack que você quer monitorados no digest diário. Inclua o nome e o ID de cada canal. Priorize canais onde:
- Decisões importantes acontecem
- Você precisa acompanhar mesmo sem ser mencionado
- Temas estratégicos são discutidos

Exemplo:
```
- #general (C0XXXXXXXXX)
- #product (C0XXXXXXXXX)
- #engineering (C0XXXXXXXXX)
```

### Radar de pares e diretos

Liste as pessoas que você quer no radar, separando entre pares (colegas de nível similar) e diretos (reportes diretos). Para cada pessoa, inclua nome e Slack user ID.

Exemplo:
```
Pares:
- Ana Silva (U0XXXXXXXXX) — VP Produto
- Carlos Lima (U0XXXXXXXXX) — VP Engenharia

Diretos:
- Marina Costa (U0XXXXXXXXX) — Product Manager
- Rafael Souza (U0XXXXXXXXX) — Tech Lead
```

### Contexto organizacional

Descreva brevemente:
- O que sua empresa faz
- Sua posição na organização
- Áreas que você supervisiona
- Temas e projetos recorrentes

Isso ajuda o Claude a priorizar informações e entender a relevância do que encontra.

### Itens pendentes

Mantenha uma lista atualizada das suas pendências atuais. O digest usa isso para detectar atualizações relevantes e cobrar progresso.

Formato sugerido:
```
- [Tema] Descrição curta — status atual
- [Tema] Descrição curta — esperando resposta de [pessoa]
```

**Dica**: atualize esta seção semanalmente ou peça ao Claude "atualiza meus itens pendentes baseado no que aconteceu essa semana".

### Lógica diário vs semanal

O template já vem com lógica para briefings diferentes:
- **Segunda a quinta**: briefing diário (foco nas últimas 24h)
- **Sexta-feira**: briefing semanal (retrospectiva da semana + preparação da próxima)

Se preferir outro padrão (ex: semanal na segunda, diário nos outros dias), ajuste a seção de lógica no SKILL.md.

---

## 3. Action Items (`action-items/SKILL.md`)

### Slack DM channel ID

Substitua `YOUR_SLACK_DM_CHANNEL_ID` pelo ID do seu canal de DM consigo mesmo (mesmo ID usado no executive-digest).

### Ferramenta de transcrição

O template assume que você usa Gemini para transcrições automáticas. Se usar outra ferramenta (Granola, Grain, Otter.ai), ajuste as queries de busca no Gmail:

```
# Gemini (padrão)
from:gemini-notes@google.com

# Granola
from:notifications@granola.ai

# Grain
from:notifications@grain.com

# Otter.ai
from:otter@otter.ai
```

---

## 4. Scheduled Tasks (`config/scheduled-tasks.md`)

### Horários

Ajuste os crons conforme sua rotina:

- **Digest**: 30 minutos antes de você começar a trabalhar
- **Meeting prep**: logo após o digest
- **Action items**: 30 minutos após seu horário típico de última reunião

Os crons usam o fuso horário local da sua máquina.

### Channel IDs nos prompts

Substitua `YOUR_SLACK_DM_CHANNEL_ID` nos 3 prompts pelo seu ID real.

---

## 5. Skills que não precisam de personalização

**`humanizer`**: funciona out-of-the-box. Usa o `_shared/style-guide.md` como referência, então basta preencher o style-guide.

**`email-drafting`**: mesma coisa — referencia o style-guide para tom e estilo.

**`meeting-prep`**: não requer personalização. Pesquisa automaticamente nos seus dados (Gmail, Calendar, Slack, Drive, web).

---

## Dicas gerais

**Itere o style-guide**: o primeiro preenchimento não vai ficar perfeito. Depois de usar as skills por uns dias, ajuste o que não soar como você.

**Teste cada skill manualmente**: antes de confiar nas scheduled tasks, rode cada skill na mão pelo menos uma vez.

**Atualize itens pendentes**: o digest é tão bom quanto a lista de pendências que você mantém atualizada.

**Ajuste canais conforme a empresa muda**: novos projetos podem criar novos canais relevantes. Revise trimestralmente.
