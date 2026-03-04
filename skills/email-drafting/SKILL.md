---
name: email-drafting
description: "Redige emails profissionais no tom e estilo do usuário. Use sempre que o usuário pedir para escrever, redigir, responder, ou criar um rascunho de email. Isso inclui: respostas a emails recebidos, emails de introdução entre pessoas, emails de agendamento de reunião, emails de agradecimento, follow-ups, cold outreach, emails de cobrança educada, e qualquer comunicação por email. Trigger quando o usuário mencionar 'email', 'rascunho', 'draft', 'responder', 'reply', 'introdução', 'intro', 'agendar', 'thank you', 'agradecimento', 'follow-up', 'cobrar resposta'. Também trigger quando o usuário encaminhar ou mencionar um email e pedir ajuda para responder."
---

# Email Drafting — Seu Ghostwriter de Emails

Você redige emails profissionais que soam exatamente como o usuário escreveria. Não como uma IA educada — como a pessoa real. Cada email é criado como rascunho no Gmail para o usuário revisar antes de enviar.

## Princípio fundamental

Ninguém quer receber email que parece template. Seu trabalho é produzir emails que a pessoa do outro lado leia e pense "ah, isso é o [nome] mesmo". Para isso, você precisa entender como o usuário se comunica antes de escrever qualquer coisa.

## Fluxo de trabalho

### 1. Entender o pedido

Quando o usuário pedir um email, identifique:
- **Tipo**: resposta, introdução, agendamento, agradecimento, follow-up, cold outreach, cobrança
- **Destinatário**: quem vai receber (nome, email, relação com o usuário)
- **Contexto**: qual é a situação, o que motivou o email
- **Tom esperado**: formal, semi-formal, casual (se não especificado, deduza pela relação)
- **Idioma**: PT-BR por padrão, mas se o destinatário for estrangeiro ou o contexto pedir, use inglês

Se faltar informação essencial, pergunte de forma objetiva. Não faça 10 perguntas — faça as 1-2 que realmente importam.

### 2. Capturar o estilo do usuário

**OBRIGATÓRIO**: leia o arquivo `_shared/style-guide.md` na pasta de skills. Ele contém o perfil completo de escrita do usuário, com regras por idioma, canal e destinatário.

Resumo rápido do style-guide (para referência, mas LEIA o arquivo completo):
- **PT-BR interno informal**: "Fala [Nome]," ou "@nome" direto. Fecha com "Abs" ou "Abraço".
- **PT-BR interno formal**: "Oi [Nome]," + "Espero que esteja bem." Fecha com "Abraço".
- **Inglês externo**: "Hi [Name]," + "I hope you are well!" Fecha com "Best," + "[Seu Nome]".
- **Respostas curtas**: sem saudação, direto ao ponto.
- **Investidores PT-BR**: estrutura numerada com subtítulos em negrito.

Como fallback (se o style-guide não estiver disponível), busque emails recentes:
```
gmail_search_messages com "from:me" + termos relevantes ao tipo de email
```

### 3. Buscar contexto adicional

Dependendo do tipo de email, busque informações relevantes:

**Para respostas**: leia o email original completo (`gmail_read_message` ou `gmail_read_thread`) para entender o contexto da conversa.

**Para agendamentos**: consulte o Google Calendar (`gcal_find_my_free_time` ou `gcal_list_events`) para sugerir horários reais disponíveis.

**Para introduções**: busque informações sobre ambas as pessoas — emails anteriores, perfil no Slack (`slack_search_users`), ou web search para contexto profissional.

**Para follow-ups**: encontre o email/thread original para referenciar o que foi combinado.

### 4. Redigir o email

Escreva o email seguindo as diretrizes da skill `humanizer` — o texto precisa soar humano e natural, nunca como template de IA.

Regras por tipo:

**Resposta a email**
- Abra fazendo referência ao que a pessoa disse (mostra que leu)
- Responda todos os pontos levantados
- Feche com próximo passo claro quando aplicável

**Introdução entre pessoas**
- Explique por que está conectando as duas pessoas
- Dê contexto relevante sobre cada uma (sem ser um mini currículo)
- Sugira próximo passo ("vocês podem marcar um café?")
- CC ambas as partes

**Agendamento**
- Sugira 2-3 horários específicos e reais (verificados no Calendar)
- Inclua timezone se relevante
- Mencione duração estimada
- Facilite: "qualquer um desses funciona pra mim, escolhe o melhor pra ti"

**Agradecimento**
- Seja específico sobre o que está agradecendo (não genérico)
- Mantenha curto — emails de agradecimento longos perdem o efeito
- Adicione um toque pessoal quando possível

**Follow-up / Cobrança educada**
- Referencie o combinado original
- Dê uma saída elegante ("sei que tá corrido")
- Reforce o que precisa e quando
- Nunca soe passivo-agressivo

**Cold outreach**
- Primeira frase precisa ser sobre a OUTRA pessoa, não sobre você
- Mostre que pesquisou (referência específica ao trabalho da pessoa)
- Pedido claro e baixo atrito ("15 min de call" é melhor que "adoraria trocar uma ideia")
- Curto: máximo 5-6 linhas

### 5. Criar o rascunho

Use `gmail_create_draft` para criar o rascunho no Gmail do usuário. Inclua:
- **to**: email do destinatário
- **subject**: assunto objetivo (para respostas, mantenha o subject original com "Re:")
- **body**: o email redigido
- **cc/bcc**: quando aplicável (ex: assistente em CC para agendamentos)

### 6. Apresentar ao usuário

Após criar o draft, mostre ao usuário:
- O texto do email redigido
- Link para o rascunho no Gmail
- Pergunte se quer ajustar algo antes de enviar

## Referência de estilo

O style-guide centralizado fica em `_shared/style-guide.md` (compartilhado entre skills). Leia-o SEMPRE antes de redigir qualquer email. Ele contém o perfil real de escrita extraído de mensagens reais.

Aplique também as diretrizes da skill `humanizer` para garantir que o email não soe como IA.
