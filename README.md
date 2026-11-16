# olm-agents

Repositório para **versionar meus agentes**. A fase atual é focada em agentes de **assistente pessoal**.

Os agentes são *subagents* do Claude Code: um arquivo Markdown por agente, em `.claude/agents/`.

## Agentes

| Agente | O que faz |
|---|---|
| [`secretaria`](.claude/agents/secretaria.md) | Secretária pessoal. Agenda, remarca e consulta compromissos no Google Calendar (evitando duplicidades e conflitos) e responde "o que preciso fazer" no dia, semana, mês, quarter e ano, cruzando agenda e plano. Também envia por e-mail ao usuário o resumo de atividades e responsabilidades. |
| [`gerente-de-projetos`](.claude/agents/gerente-de-projetos.md) | Gerente de projetos sênior. Transforma um objetivo grande em plano factível e o mantém vivo, em 4 modos: brainstorm, roadmap, análise semanal e remanejamento do dia. Também gera o Gantt e relatórios por período. |

### Como os dois se relacionam

- **`gerente-de-projetos`** cuida do plano: a árvore de objetivos (5 anos → 1 ano → quarter → mês → tarefas da semana) fica no **Notion**, e as tarefas acionáveis viram cards no **Trello**.
- **`secretaria`** cuida da agenda (**Google Calendar**) e é a ponte entre o usuário e o plano: lê os cards do Trello, consulta o `gerente-de-projetos` quando precisa de relatórios e sempre cruza com a agenda. Ela só lê o plano; criar ou mover cards é papel do gerente.

### Integrações necessárias

Os agentes usam ferramentas MCP do Claude, que precisam estar conectadas na sua conta:

- Google Calendar (`secretaria`, `gerente-de-projetos`)
- Gmail (`secretaria`, para enviar o resumo por e-mail)
- Trello (`secretaria` lê; `gerente-de-projetos` lê e escreve)
- Notion (`gerente-de-projetos`)

## Estrutura

```
olm-agents/
├── .claude/
│   ├── agents/
│   │   ├── secretaria.md
│   │   └── gerente-de-projetos.md
│   └── settings.local.json   # configuração local (permissões)
├── CLAUDE.md
└── README.md
```

Agentes programados (por exemplo, com o Agent SDK ou a API) ainda não existem; a pasta para eles será definida quando o primeiro surgir.

## Como usar

Abra o Claude Code dentro da pasta `olm-agents/`. Os agentes do projeto ficam disponíveis na sessão e são acionados automaticamente pela `description` de cada um, ou explicitamente pelo nome. Se existir um agente com o mesmo nome em `~/.claude/agents/`, o do projeto tem precedência.

## Como adicionar um agente

1. Crie `.claude/agents/<nome-do-agente>.md`.
2. Preencha o frontmatter e escreva o prompt no corpo:

   ```markdown
   ---
   name: nome-do-agente
   description: Quando este agente deve ser usado (uma frase objetiva).
   ---

   Instruções do agente aqui.
   ```

3. Adicione o agente à tabela de [Agentes](#agentes) acima e faça o commit.

## Convenções dos agentes atuais

- Respondem em português do Brasil, com datas no formato DD/MM/AAAA e horários em 24h, no fuso America/Sao_Paulo.
- Não criam, alteram nem excluem nada (eventos, cards, páginas) sem aprovação do usuário.
- Quando rodam como subagent, devolvem as pendências e perguntas como resultado final em vez de ficarem bloqueados esperando resposta.
