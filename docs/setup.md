# Setup Guide — Instalação Completa

## Pré-requisitos

1. **Claude Desktop** com Cowork mode ativo
2. **Conectores habilitados** no Cowork:
   - Gmail (ler, buscar, criar rascunhos, enviar)
   - Google Calendar (ler, criar, editar eventos)
   - Slack (buscar, ler, enviar mensagens)
   - Google Drive (buscar, ler documentos) — opcional
3. **Transcrição de reuniões** (opcional, para a skill action-items):
   - Gemini, Granola, Grain, Otter.ai, ou qualquer ferramenta que salve transcrições no Gmail/Drive
   - Se não usar transcrição automática, pode fazer upload manual das notas

## Instalação das Skills

### Opção 1 — Copiar para a pasta de skills do Cowork

Localize a pasta de skills do seu Cowork. No macOS:

```bash
# A pasta de skills geralmente fica em:
~/Library/Application Support/Claude/skills/

# Ou verifique no Cowork perguntando:
# "Onde fica a pasta de skills?"
```

Copie as pastas de skills:

```bash
git clone https://github.com/YOUR_USERNAME/cowork-executive-assistant.git
cp -r cowork-executive-assistant/skills/* ~/Library/Application\ Support/Claude/skills/
```

### Opção 2 — Pedir ao Claude para instalar

Abra o Cowork e diga:

```
Instala essas skills do meu repositório: [link do repo]
```

### Opção 3 — Criar manualmente

Para cada skill, abra o Cowork e diga:

```
Cria uma skill chamada "humanizer" com este conteúdo: [cole o SKILL.md]
```

## Configuração pós-instalação

### 1. Personalizar o style-guide (centralizado)

Edite o arquivo `skills/_shared/style-guide.md` com:
- Suas saudações típicas por canal e idioma
- Seu fechamento de email/Slack preferido
- Características do seu estilo de escrita
- Anti-padrões (o que NUNCA escrever no seu nome)

Este arquivo é compartilhado por todas as skills que escrevem texto na sua voz (humanizer, email-drafting, action-items).

Dica: peça ao Claude *"analisa meus últimos 20 emails enviados e mensagens no Slack, e preenche o style guide pra mim"*.

### 2. Personalizar o executive-digest

O executive-digest é a skill mais personalizada. Edite `skills/executive-digest/SKILL.md` e preencha:

- **Dados do usuário**: Slack user ID, DM channel ID, email, timezone
- **Canais diários**: os canais Slack que você quer monitorados todo dia
- **Pares e diretos**: as pessoas que você quer no radar, com Slack user IDs
- **Contexto organizacional**: estrutura da empresa, áreas, temas recorrentes
- **Itens pendentes**: suas pendências atuais

Procure os comentários `<!-- PERSONALIZAR -->` no arquivo.

### 3. Personalizar o action-items

Edite `skills/action-items/SKILL.md` e substitua `YOUR_SLACK_DM_CHANNEL_ID` pelo ID do seu canal de DM.

### 4. Configurar scheduled tasks

Veja `config/scheduled-tasks.md` para prompts prontos. Configure as tasks que quiser:

```
Cria uma scheduled task chamada "executive-digest"
que roda todo dia de semana às 8h com este prompt: [prompt do config]
```

### 5. Definir canal de entrega

Cada skill pode entregar resultados via:
- **Slack DM**: mensagem direta para você no Slack
- **Gmail draft**: rascunho no seu Gmail
- **Cowork**: diretamente na conversa (quando usado sob demanda)

Ajuste os prompts das scheduled tasks conforme sua preferência.

## Verificação

Teste cada skill manualmente:

1. **humanizer**: "Humaniza esse texto: Certamente! Gostaria de informar que o projeto está progredindo conforme planejado."
2. **email-drafting**: "Redige um email de agradecimento para [nome] pela reunião de hoje"
3. **meeting-prep**: "Me prepara para minha próxima reunião"
4. **action-items**: "Revisa as reuniões de hoje e lista os follow-ups"
5. **executive-digest**: "Me dá o digest de hoje"

Se alguma skill não for reconhecida, verifique se está na pasta correta e reinicie o Cowork.

## Solução de problemas

**Skill não é reconhecida**: verifique se o arquivo SKILL.md está na pasta correta e se o frontmatter (entre `---`) está bem formatado.

**Gmail/Calendar não responde**: verifique se os conectores estão habilitados em Settings > Connectors no Cowork.

**Transcrições não encontradas**: verifique se a ferramenta de transcrição salva no Gmail/Drive e ajuste as queries de busca se necessário.

**Scheduled task não roda**: verifique se o Cowork está aberto no horário do cron. Tasks só executam com o app rodando.

**Slack DM channel ID**: para encontrar, abra seu DM consigo mesmo no Slack, clique no nome do canal no topo, e copie o Channel ID (começa com `D`).
