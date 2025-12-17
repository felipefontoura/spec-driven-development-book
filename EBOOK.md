# Spec-Driven Development

## O Guia Definitivo para Construir Produtos com IA

### Construindo o TaskFlow Pro do Zero

---

**Por que este livro existe**

Você é um desenvolvedor experiente. Já construiu sistemas, debugou código às 3h da manhã, e sabe que a parte mais difícil de qualquer projeto não é escrever código — é saber *o que* escrever.

Com a chegada de agentes de IA como Claude Code, Cursor e Copilot, essa verdade se tornou ainda mais evidente. Essas ferramentas podem gerar milhares de linhas de código em minutos, mas sem direção clara, produzem o equivalente digital de um castelo de cartas: impressionante à primeira vista, mas pronto para desabar ao primeiro sopro de requisito mal entendido.

Este livro vai te ensinar **Spec-Driven Development (SDD)** — uma metodologia que transforma a maneira como você trabalha com agentes de IA. Vamos construir juntos uma aplicação real: o TaskFlow Pro, um sistema de gerenciamento de tarefas colaborativo com workspaces, automações e notificações em tempo real.

Ao final, você terá não apenas o conhecimento teórico, mas especificações completas e prontas para usar.

---

# PARTE I: FUNDAMENTOS

---

## Capítulo 1: O Problema que Ninguém Fala

### 1.1 A Ilusão da Velocidade

Imagine que você contratou um pedreiro extraordinariamente rápido. Ele ergue paredes em minutos, instala encanamentos em segundos, e termina o telhado antes do almoço. Impressionante, certo?

Agora imagine que você esqueceu de dar a planta da casa para ele.

O resultado? Uma construção que pode até ficar de pé, mas com o banheiro no lugar da cozinha, portas que abrem para paredes, e uma escada que leva a lugar nenhum.

**Este é exatamente o problema com desenvolvimento assistido por IA sem especificações.**

Claude Code, Cursor, e similares são esses pedreiros extraordinários. Eles podem gerar código a uma velocidade que seria ficção científica há cinco anos. Mas velocidade sem direção é apenas uma forma mais rápida de chegar ao lugar errado.

### 1.2 O Custo Real do Retrabalho

Vamos fazer uma conta simples. Em um projeto típico sem especificações:

```
Iteração 1: "Crie um sistema de tarefas"
→ Claude gera código
→ Você percebe que faltou workspaces
→ 15 minutos perdidos

Iteração 2: "Adicione workspaces"
→ Claude modifica, quebra algo
→ Você percebe que precisa de permissões por workspace
→ 20 minutos perdidos

Iteração 3: "Adicione sistema de permissões"
→ Claude refatora
→ Você percebe que não pensou em convites
→ 30 minutos perdidos

... e assim por diante
```

Em contraste, com especificações:

```
Especificação completa: 2 horas de planejamento
→ Claude gera código seguindo spec
→ Ajustes menores
→ Implementação correta de primeira

Tempo total: 3 horas
vs.
Tempo sem spec: 8-12 horas (conservadoramente)
```

A matemática é clara: **investir tempo em especificações economiza tempo total**.

### 1.3 Por Que Desenvolvedores Experientes Resistem

Se você tem 10, 15, 20 anos de experiência, provavelmente está pensando: "Eu já sei o que preciso construir. Especificações formais são burocracia."

Eu entendo. Por anos, as melhores práticas de desenvolvimento enfatizaram agilidade, iteração rápida, e "código funcionando sobre documentação abrangente".

Mas aqui está a diferença crucial: **você não está mais codando sozinho**.

Quando você escreve código, seu cérebro mantém um modelo mental do sistema inteiro. Você sabe que aquela função obscura em `utils.ts` é crítica porque lembra da vez que ela salvou o projeto às 2h da manhã.

Claude não tem essa memória. Cada sessão começa do zero. Cada contexto é limitado. O agente é extraordinariamente capaz, mas opera em um presente eterno, sem acesso às decisões que você tomou ontem ou às lições que aprendeu no mês passado.

**Especificações são a memória externa que agentes de IA precisam.**

### 1.4 A Analogia do GPS

Pense em especificações como um GPS para sua jornada de desenvolvimento.

Sem GPS (sem specs):

- Você sabe vagamente onde quer chegar
- Toma decisões no momento
- Às vezes encontra atalhos
- Frequentemente se perde
- Gasta combustível (tempo) desnecessário

Com GPS (com specs):

- Destino claramente definido
- Rota calculada considerando obstáculos
- Recálculo automático quando algo muda
- Chegada previsível

O GPS não tira sua autonomia como motorista. Você ainda decide quando parar, qual música ouvir, e pode ignorar sugestões se souber de algo melhor. Mas ele garante que você não vai acabar em outro estado quando queria ir ao supermercado.

---

## Capítulo 2: O Que é Spec-Driven Development

### 2.1 Definição Formal

**Spec-Driven Development (SDD)** é uma metodologia de desenvolvimento de software onde especificações detalhadas são criadas e aprovadas *antes* de qualquer implementação, servindo como contrato entre humanos e agentes de IA.

Diferente de metodologias tradicionais onde documentação é um artefato secundário, em SDD a especificação é o artefato primário. O código é uma consequência da spec, não o contrário.

### 2.2 Os Quatro Pilares

SDD se sustenta em quatro pilares fundamentais:

```mermaid
flowchart TB
    subgraph SDD["SPEC-DRIVEN DEVELOPMENT"]
        REQ["📋 REQUIREMENTS<br/>(O QUÊ)"]
        DES["🏗️ DESIGN<br/>(COMO)"]
        TSK["📝 TASKS<br/>(QUANTO)"]
        IMP["⚙️ IMPLEMENTATION<br/>(EXECUÇÃO)"]
        
        REQ --> DES
        DES --> TSK
        TSK --> IMP
        IMP -.->|feedback| REQ
    end
    
    style REQ fill:#3b82f6,color:#fff
    style DES fill:#8b5cf6,color:#fff
    style TSK fill:#f59e0b,color:#fff
    style IMP fill:#10b981,color:#fff
```

**Pilar 1: Requirements (Requisitos)**

- Define O QUÊ o sistema deve fazer
- Escrito em linguagem de negócio
- Focado em comportamentos observáveis
- Independente de tecnologia

**Pilar 2: Design (Projeto)**

- Define COMO o sistema vai funcionar
- Decisões arquiteturais
- Escolhas tecnológicas justificadas
- Modelos de dados e APIs

**Pilar 3: Tasks (Tarefas)**

- Define QUANTO trabalho existe
- Decomposição em unidades implementáveis
- Dependências e prioridades
- Rastreabilidade para requisitos

**Pilar 4: Implementation (Implementação)**

- EXECUÇÃO seguindo as specs
- Verificação contra critérios de aceitação
- Documentação de decisões de implementação
- Feedback para specs futuras

### 2.3 O Fluxo de Gates

Um conceito crucial em SDD é o sistema de **gates** (portões) entre fases:

```mermaid
flowchart LR
    REQ[Requirements] -->|"🚪 GATE 1"| DES[Design]
    DES -->|"🚪 GATE 2"| TSK[Tasks]
    TSK -->|"🚪 GATE 3"| COD[Code]
    
    REQ ---|"✅ Aprovação<br/>Humana"| G1[" "]
    DES ---|"✅ Aprovação<br/>Humana"| G2[" "]
    TSK ---|"✅ Aprovação<br/>Humana"| G3[" "]
    
    style REQ fill:#3b82f6,color:#fff
    style DES fill:#8b5cf6,color:#fff
    style TSK fill:#f59e0b,color:#fff
    style COD fill:#10b981,color:#fff
    style G1 fill:transparent,stroke:transparent
    style G2 fill:transparent,stroke:transparent
    style G3 fill:transparent,stroke:transparent
```

Cada gate representa um ponto de decisão humana. O agente de IA **não pode** avançar para a próxima fase sem aprovação explícita. Isso garante que:

1. Erros são detectados antes de se propagarem
2. Decisões críticas são revisadas por humanos
3. O custo de correção é minimizado (quanto mais cedo, mais barato)

### 2.4 Comparação com Outras Metodologias

| Aspecto | Waterfall | Agile/Scrum | SDD |
|---------|-----------|-------------|-----|
| Documentação | Extensiva upfront | Mínima | Estruturada por fase |
| Flexibilidade | Baixa | Alta | Média-alta |
| Feedback loops | Longos | Curtos | Por fase |
| Adequação para IA | Ruim | Razoável | Excelente |
| Overhead | Alto | Baixo | Médio |
| Rastreabilidade | Alta | Baixa | Alta |

SDD não é Waterfall disfarçado. A diferença crucial é que specs em SDD são **vivas** — elas evoluem, mas de forma controlada. Você pode voltar e modificar requisitos, mas essa modificação se propaga conscientemente através de design e tasks.

### 2.5 Quando Usar (e Quando Não Usar)

**Use SDD quando:**

- O projeto dura mais que alguns dias
- Múltiplas features complexas estão envolvidas
- Você trabalha com agentes de IA
- A arquitetura precisa ser pensada com cuidado
- Rastreabilidade é importante
- Múltiplas sessões de desenvolvimento serão necessárias

**Considere alternativas quando:**

- É um script simples de uma hora
- Você está prototipando para descobrir requisitos
- O escopo é trivial e bem conhecido
- Você vai implementar tudo em uma única sessão

Para nosso projeto TaskFlow Pro, SDD é a escolha óbvia: temos autenticação, workspaces colaborativos, sistema de permissões, automações, notificações em tempo real — complexidade que exige planejamento.

---

## Capítulo 3: Anatomia de uma Especificação

### 3.1 A Estrutura de Diretórios

Antes de escrever qualquer spec, precisamos de uma estrutura organizacional clara:

```mermaid
graph TD
    ROOT[📁 projeto/] --> CLAUDE_DIR[📁 .claude/]
    ROOT --> CLAUDE_MD[📄 CLAUDE.md]
    ROOT --> APPS[📁 apps/]
    ROOT --> PACKAGES[📁 packages/]
    
    CLAUDE_DIR --> COMMANDS[📁 commands/]
    CLAUDE_DIR --> STEERING[📁 steering/]
    CLAUDE_DIR --> SPECS[📁 specs/]
    CLAUDE_DIR --> AGENTS[📁 agents/]
    
    COMMANDS --> C1[📄 spec-new.md]
    COMMANDS --> C2[📄 spec-requirements.md]
    COMMANDS --> C3[📄 spec-design.md]
    
    STEERING --> S1[📄 product.md]
    STEERING --> S2[📄 tech-stack.md]
    STEERING --> S3[📄 conventions.md]
    
    SPECS --> SP1[📁 001-auth/]
    SPECS --> SP2[📁 002-workspaces/]
    SPECS --> SP3[📁 003-tasks/]
    
    SP1 --> SP1A[📄 requirements.md]
    SP1 --> SP1B[📄 design.md]
    SP1 --> SP1C[📄 tasks.md]
    
    AGENTS --> A1[📄 architect.md]
    AGENTS --> A2[📄 implementer.md]
    AGENTS --> A3[📄 reviewer.md]
    
    style ROOT fill:#1e293b,color:#fff
    style CLAUDE_DIR fill:#3b82f6,color:#fff
    style SPECS fill:#8b5cf6,color:#fff
```

### 3.2 O Arquivo CLAUDE.md

O `CLAUDE.md` é o ponto de entrada. Ele deve ser conciso (< 300 linhas) e usar imports para detalhes:

```markdown
# TaskFlow Pro - Instruções para Claude

## Sobre o Projeto
Sistema de gerenciamento de tarefas colaborativo com workspaces,
automações e notificações em tempo real.
Veja @.claude/steering/product.md para detalhes completos.

## Stack Tecnológica
- Monorepo: Turborepo
- Frontend: Next.js 14 (App Router) + shadcn/ui
- Backend: Fastify + Prisma
- Real-time: Socket.io
- Queue: BullMQ + Redis

Detalhes em @.claude/steering/tech-stack.md

## Convenções
Veja @.claude/steering/conventions.md

## Workflow de Desenvolvimento
Este projeto usa Spec-Driven Development.

### Antes de implementar qualquer feature:
1. Verifique se existe spec em `.claude/specs/`
2. Leia requirements.md, design.md, e tasks.md
3. Implemente seguindo a spec
4. Marque tasks como completas

### Para criar nova feature:
1. Use `/spec-new [nome-feature]`
2. Siga o workflow: requirements → design → tasks → implementation
3. Aguarde aprovação humana entre cada fase

## Regras Críticas
- NUNCA exponha dados de um workspace para outro
- SEMPRE verifique permissões antes de operações
- SEMPRE use transações para operações que afetam múltiplas tabelas
- NUNCA confie em dados do cliente - valide no servidor

## Links Úteis
- Documentação Prisma: @docs/prisma.md
- Guia de APIs: @docs/api-guide.md
```

### 3.3 Anatomia de requirements.md

O arquivo de requisitos é escrito em linguagem de negócio, não técnica:

```markdown
# Feature: Gerenciamento de Tarefas

## Visão Geral
Permitir que usuários criem, organizem e gerenciem tarefas
dentro de workspaces, com suporte a subtasks, tags, datas
de vencimento e atribuições.

## Contexto de Negócio
Tarefas são o core do produto. Um sistema de tarefas bem
projetado é a base para features mais avançadas como
automações e integrações com calendário.

## User Stories

### US-001: Criar Tarefa
**Como** membro de um workspace
**Quero** criar uma nova tarefa
**Para que** possa registrar trabalho a ser feito

**Critérios de Aceitação:**
- [ ] Tarefa criada com título obrigatório
- [ ] Descrição opcional em markdown
- [ ] Data de vencimento opcional
- [ ] Pode atribuir a membros do workspace
- [ ] Pode adicionar tags existentes ou criar novas
- [ ] Tarefa aparece na lista imediatamente

### US-002: Criar Subtask
**Como** membro de um workspace
**Quero** criar subtasks dentro de uma tarefa
**Para que** possa quebrar trabalho complexo em partes menores

**Critérios de Aceitação:**
- [ ] Subtask tem título obrigatório
- [ ] Subtask herda workspace da tarefa pai
- [ ] Subtask pode ser marcada como concluída independentemente
- [ ] Progresso da tarefa pai reflete % de subtasks concluídas
- [ ] Máximo de 50 subtasks por tarefa

### US-003: Completar Tarefa
**Como** membro de um workspace
**Quero** marcar uma tarefa como concluída
**Para que** possa acompanhar meu progresso

**Critérios de Aceitação:**
- [ ] Um clique para marcar como concluída
- [ ] Tarefa concluída move para seção "Concluídas"
- [ ] Data/hora de conclusão registrada
- [ ] Pode desfazer conclusão
- [ ] Notifica membros atribuídos (se configurado)

## Requisitos Funcionais

### RF-001 (Must Have)
O sistema DEVE validar que o usuário tem permissão no workspace
antes de criar/editar/deletar tarefas.

### RF-002 (Must Have)
O sistema DEVE atualizar a lista de tarefas em tempo real
para todos os membros do workspace quando houver mudanças.

### RF-003 (Must Have)
O sistema DEVE manter histórico de alterações em cada tarefa
(quem alterou, quando, o quê mudou).

### RF-004 (Should Have)
O sistema DEVE permitir arrastar e soltar para reordenar tarefas.

### RF-005 (Could Have)
O sistema PODE sugerir tags baseado no título da tarefa.

## Requisitos Não-Funcionais

### RNF-001: Performance
- Lista de tarefas: carregamento < 500ms
- Criar tarefa: resposta < 300ms
- Atualização real-time: latência < 200ms

### RNF-002: Escalabilidade
- Suportar até 10.000 tarefas por workspace
- Suportar até 100 membros por workspace

### RNF-003: Usabilidade
- Interface responsiva (mobile-first)
- Atalhos de teclado para ações comuns
- Feedback visual para todas as ações

## Glossário
- **Workspace**: Espaço de trabalho compartilhado por uma equipe
- **Task**: Unidade de trabalho a ser realizada
- **Subtask**: Subdivisão de uma task
- **Tag**: Etiqueta para categorização de tarefas
- **Assignee**: Membro atribuído a uma tarefa

## Referências
- Benchmark: Todoist, Linear, Asana
- Design System: @.claude/steering/design-system.md
```

### 3.4 Usando o Formato EARS

EARS (Easy Approach to Requirements Syntax) é um formato que ajuda a escrever requisitos não-ambíguos:

```markdown
## Requisitos Funcionais (Formato EARS)

### Ubiquitous (Sempre verdadeiro)
O sistema DEVE validar permissões de workspace em todas 
as operações de tarefas.

### Event-Driven (Quando algo acontece)
QUANDO uma tarefa é marcada como concluída, 
o sistema DEVE registrar timestamp e usuário que concluiu.

### Unwanted Behavior (Tratamento de erro)
SE o usuário tentar criar mais de 50 subtasks,
o sistema DEVE exibir erro "Limite de subtasks atingido".

### State-Driven (Baseado em estado)
ENQUANTO uma tarefa estiver arquivada,
o sistema NÃO DEVE permitir edições.

### Optional (Condicional)
ONDE o workspace tiver notificações habilitadas,
o sistema DEVE notificar assignees quando tarefa for modificada.

### Complex (Combinação)
QUANDO uma tarefa for concluída
E a tarefa tiver automação configurada,
o sistema DEVE executar a automação
ANTES de atualizar o status para concluída.
```

---

## Capítulo 4: Escrevendo Specs Efetivas

### 4.1 O Princípio da Criança Inteligente

Imagine que você está explicando seu sistema para uma criança muito inteligente de 12 anos. Ela é esperta, faz boas perguntas, e consegue entender conceitos complexos — mas não tem seu contexto implícito.

Você não diria: "Faz aquele negócio das tasks ali".

Você diria: "Quando alguém cria uma tarefa, precisamos salvar o título, verificar se a pessoa tem permissão naquele workspace, e avisar todo mundo que está olhando a lista que uma tarefa nova apareceu."

**Este é exatamente o nível de clareza que suas specs precisam ter.**

### 4.2 Técnicas de Escrita

#### Técnica 1: Seja Específico, Não Genérico

```markdown
# ❌ Ruim
O sistema deve ser rápido.

# ✅ Bom
O endpoint GET /api/v1/tasks deve responder em menos de 
500ms no percentil 95 (p95) para listas de até 1000 tarefas.
```

#### Técnica 2: Defina o Escopo Negativo

```markdown
# ✅ Bom - Define o que NÃO fazer
## Fora do Escopo
- Este MVP não suporta tarefas recorrentes
- Não há integração com Google Calendar (será v2)
- Não há funcionalidade de time tracking
- Não há dependências entre tarefas (apenas subtasks)
```

#### Técnica 3: Use Exemplos Concretos

```markdown
# ❌ Ruim
Validar o título da tarefa.

# ✅ Bom
## Validação de Título

| Cenário | Input | Resultado |
|---------|-------|-----------|
| Vazio | "" | Erro: "Título é obrigatório" |
| Muito curto | "A" | Erro: "Título deve ter pelo menos 2 caracteres" |
| Válido | "Revisar PR #123" | Sucesso |
| Muito longo | "A" * 501 | Erro: "Título deve ter no máximo 500 caracteres" |
| Com emojis | "🚀 Deploy v2" | Sucesso |
| Só espaços | "   " | Erro: "Título é obrigatório" |
```

#### Técnica 4: Antecipe Perguntas

```markdown
## FAQ de Implementação

**P: O que acontece se deletar uma tarefa com subtasks?**
R: Subtasks são deletadas em cascata. Confirmar com usuário antes.

**P: Quem pode ver tarefas arquivadas?**
R: Todos os membros do workspace. Arquivadas ficam em aba separada.

**P: Tarefa sem assignee aparece para quem?**
R: Para todos os membros do workspace na lista principal.

**P: Como ordenar tarefas por padrão?**
R: Por data de criação (mais recentes primeiro), depois por prioridade.
```

---

## Capítulo 5: O Projeto TaskFlow Pro

### 5.1 Visão do Produto

```markdown
# .claude/steering/product.md

# TaskFlow Pro - Visão do Produto

## Proposta de Valor
TaskFlow Pro é um sistema de gerenciamento de tarefas colaborativo
que permite equipes organizarem trabalho com workspaces dedicados,
automações inteligentes e sincronização em tempo real.

## Problema que Resolvemos
1. Equipes precisam de espaços organizados por projeto/cliente
2. Tarefas repetitivas consomem tempo sem automação
3. Falta de visibilidade em tempo real causa retrabalho
4. Sistemas existentes são complexos demais ou simples demais

## Solução
- Workspaces isolados com controle de acesso
- Sistema de tarefas flexível com subtasks e tags
- Automações configuráveis (quando X acontece, faça Y)
- Atualizações em tempo real via WebSocket
- Integração com calendário para due dates

## Usuários-Alvo
1. **Pequenas equipes (3-10)**: Startups, agências
2. **Freelancers**: Gerenciando múltiplos clientes
3. **Times de produto**: Acompanhando features e bugs

## Funcionalidades Core (MVP)
1. Autenticação (email/senha, magic link)
2. Workspaces com convites e roles
3. Tarefas com subtasks, tags, due dates, assignees
4. Notificações em tempo real
5. Automações simples (quando completa X, cria Y)

## Funcionalidades Futuras (v2+)
- Integração Google Calendar
- Tarefas recorrentes
- Kanban boards
- Time tracking
- API pública

## Métricas de Sucesso
- 1000 usuários ativos em 3 meses
- Retenção D7 > 40%
- NPS > 50
```

### 5.2 Stack Tecnológica

```markdown
# .claude/steering/tech-stack.md

# Stack Tecnológica - TaskFlow Pro
```

```mermaid
flowchart TB
    subgraph TURBOREPO["🏗️ TURBOREPO"]
        subgraph APPS["📦 apps/"]
            WEB["🌐 web<br/>Next.js 14"]
            API["⚡ api<br/>Fastify"]
        end
        
        subgraph PACKAGES["📦 packages/"]
            UI["🎨 ui<br/>shadcn/ui"]
            DB["🗄️ database<br/>Prisma"]
            UTILS["🔧 utils"]
            EMAIL["📧 email<br/>React Email"]
            RT["📡 realtime<br/>Socket.io types"]
        end
        
        subgraph TOOLING["🛠️ tooling/"]
            ESL["eslint-config"]
            TSC["typescript-config"]
            TWC["tailwind-config"]
        end
    end
    
    WEB --> UI
    WEB --> UTILS
    API --> DB
    API --> UTILS
    API --> EMAIL
    
    style TURBOREPO fill:#1e293b,color:#fff
    style APPS fill:#3b82f6,color:#fff
    style PACKAGES fill:#8b5cf6,color:#fff
    style TOOLING fill:#64748b,color:#fff
```

## Apps

### apps/web (Frontend)

- **Framework:** Next.js 14 (App Router)
- **Styling:** Tailwind CSS + shadcn/ui
- **Estado:** React Query (server state) + Zustand (client state)
- **Forms:** React Hook Form + Zod
- **Real-time:** Socket.io client
- **DnD:** @dnd-kit

**Justificativa:** Next.js oferece SSR/SSG, App Router é o futuro,
shadcn/ui dá componentes acessíveis sem lock-in.

### apps/api (Backend)

- **Framework:** Fastify
- **ORM:** Prisma
- **Validação:** Zod + @fastify/type-provider-zod
- **Auth:** @fastify/jwt + magic links
- **Real-time:** Socket.io server
- **Queue:** BullMQ

**Justificativa:** Fastify é mais rápido que Express, tem excelente
suporte a TypeScript, e plugin ecosystem maduro.

## Versões Fixadas

```json
{
  "dependencies": {
    "next": "14.1.0",
    "react": "18.2.0",
    "fastify": "4.26.0",
    "prisma": "5.9.0",
    "@prisma/client": "5.9.0",
    "socket.io": "4.7.0",
    "bullmq": "5.1.0",
    "zod": "3.22.4",
    "@tanstack/react-query": "5.17.0"
  }
}
```

## Decisões Arquiteturais

### Por que Turborepo?

- Caching inteligente acelera builds
- Workspace dependencies facilitam refactoring
- Task orchestration para CI/CD

### Por que Fastify ao invés de Express?

- 2-3x mais rápido
- Suporte nativo a async/await
- Schema validation built-in
- Melhor TypeScript support

### Por que Socket.io ao invés de WebSocket puro?

- Fallback automático para polling
- Rooms para workspaces
- Reconexão automática
- Melhor DX

### Por que BullMQ para automações?

- Retry automático
- Delayed jobs
- Rate limiting
- Dashboard (Bull Board)

---

# PARTE II: SPECS COMPLETAS DO PROJETO

---

## Capítulo 6: Spec de Autenticação

```markdown
# .claude/specs/001-auth/requirements.md

# Feature: Autenticação de Usuários

## Visão Geral
Sistema de autenticação para usuários acessarem o TaskFlow Pro,
com suporte a email/senha e magic links.

## User Stories

### US-001: Registro com Email/Senha
**Como** novo usuário
**Quero** criar uma conta com email e senha
**Para que** possa acessar o sistema

**Critérios de Aceitação:**
- [ ] Formulário: nome, email, senha
- [ ] Email único no sistema
- [ ] Senha: mínimo 8 chars, 1 maiúscula, 1 número
- [ ] Email de confirmação enviado
- [ ] Conta ativa após confirmar email

### US-002: Login com Email/Senha
**Como** usuário registrado
**Quero** fazer login com minhas credenciais
**Para que** possa acessar meus workspaces

**Critérios de Aceitação:**
- [ ] Login com email + senha
- [ ] Máximo 5 tentativas antes de bloquear (15 min)
- [ ] Opção "lembrar-me" (30 dias)
- [ ] Redirect para último workspace acessado

### US-003: Login com Magic Link
**Como** usuário
**Quero** fazer login apenas com meu email
**Para que** não precise lembrar senha

**Critérios de Aceitação:**
- [ ] Inserir apenas email
- [ ] Receber link por email (válido 15 min)
- [ ] Um clique no link faz login
- [ ] Link de uso único

### US-004: Recuperação de Senha
**Como** usuário que esqueceu a senha
**Quero** redefinir minha senha
**Para que** possa voltar a acessar minha conta

**Critérios de Aceitação:**
- [ ] Solicitar reset via email
- [ ] Link válido por 1 hora
- [ ] Link de uso único
- [ ] Notificação quando senha alterada

## Requisitos Funcionais

### RF-001 (Must Have)
O sistema DEVE armazenar senhas usando bcrypt com cost factor 12.

### RF-002 (Must Have)
O sistema DEVE usar JWT com expiração de 1 hora e refresh token de 7 dias.

### RF-003 (Must Have)
O sistema DEVE invalidar todos os refresh tokens quando senha for alterada.

### RF-004 (Should Have)
O sistema DEVE registrar todas as tentativas de login para auditoria.

## Requisitos Não-Funcionais

### RNF-001: Performance
- Login: < 1 segundo
- Registro: < 2 segundos

### RNF-002: Segurança
- HTTPS obrigatório
- Cookies com Secure, HttpOnly, SameSite
- Rate limiting: 10 logins/minuto por IP
```

```markdown
# .claude/specs/001-auth/design.md

# Design: Autenticação
```

### Fluxo de Autenticação

```mermaid
sequenceDiagram
    participant U as 👤 Usuário
    participant F as 🌐 Frontend
    participant A as ⚡ API
    participant D as 🗄️ Database
    participant E as 📧 Email
    
    Note over U,E: Registro
    U->>F: Preenche formulário
    F->>A: POST /auth/register
    A->>D: Criar usuário
    A->>E: Enviar email verificação
    A->>F: 201 Created
    F->>U: "Verifique seu email"
    
    Note over U,E: Login
    U->>F: Email + Senha
    F->>A: POST /auth/login
    A->>D: Verificar credenciais
    A->>D: Criar session
    A->>F: accessToken + refreshToken
    F->>U: Redirect para dashboard
```

## Modelo de Dados

```prisma
model User {
  id              String    @id @default(cuid())
  name            String
  email           String    @unique
  passwordHash    String?   // Null se usar apenas magic link
  emailVerified   Boolean   @default(false)
  emailVerifiedAt DateTime?
  avatarUrl       String?
  
  sessions        Session[]
  workspaceMembers WorkspaceMember[]
  
  createdAt       DateTime  @default(now())
  updatedAt       DateTime  @updatedAt
  lastLoginAt     DateTime?
}

model Session {
  id              String    @id @default(cuid())
  userId          String
  user            User      @relation(fields: [userId], references: [id])
  
  refreshToken    String    @unique
  userAgent       String?
  ipAddress       String?
  
  expiresAt       DateTime
  createdAt       DateTime  @default(now())
  
  @@index([userId])
  @@index([refreshToken])
}

model MagicLink {
  id              String    @id @default(cuid())
  email           String
  token           String    @unique
  expiresAt       DateTime
  usedAt          DateTime?
  
  createdAt       DateTime  @default(now())
  
  @@index([token])
  @@index([email])
}

model PasswordReset {
  id              String    @id @default(cuid())
  userId          String
  token           String    @unique
  expiresAt       DateTime
  usedAt          DateTime?
  
  createdAt       DateTime  @default(now())
  
  @@index([token])
}
```

## API Endpoints

```yaml
POST /api/v1/auth/register
  Body: { name, email, password }
  Response 201: { message: "Verifique seu email" }

POST /api/v1/auth/login
  Body: { email, password, remember? }
  Response 200: { accessToken, refreshToken, user }

POST /api/v1/auth/magic-link
  Body: { email }
  Response 200: { message: "Link enviado" }

POST /api/v1/auth/magic-link/verify
  Body: { token }
  Response 200: { accessToken, refreshToken, user }

POST /api/v1/auth/refresh
  Cookie: refreshToken
  Response 200: { accessToken }

POST /api/v1/auth/logout
  Authorization: Bearer {token}
  Response 204

POST /api/v1/auth/forgot-password
  Body: { email }
  Response 200: { message: "Email enviado se conta existir" }

POST /api/v1/auth/reset-password
  Body: { token, password }
  Response 200: { message: "Senha alterada" }

GET /api/v1/auth/me
  Authorization: Bearer {token}
  Response 200: { user }
```

## JWT Structure

```typescript
interface JWTPayload {
  sub: string;        // userId
  email: string;
  name: string;
  iat: number;
  exp: number;
}
```

## Segurança

### Password Hashing

```typescript
import bcrypt from 'bcrypt';
const SALT_ROUNDS = 12;

async function hashPassword(password: string): Promise<string> {
  return bcrypt.hash(password, SALT_ROUNDS);
}
```

### Rate Limiting

```typescript
const rateLimiter = {
  loginByEmail: { points: 5, duration: 900 },
  loginByIP: { points: 10, duration: 900 },
  magicLinkByEmail: { points: 3, duration: 3600 }
};
```

```markdown
# .claude/specs/001-auth/tasks.md

# Tasks: Autenticação

## Fase 1: Backend - Models (0.5 dia)

### Task 1.1: Schema Prisma
**Estimativa:** 1.5h
- [ ] Model User
- [ ] Model Session
- [ ] Model MagicLink
- [ ] Model PasswordReset
- [ ] Migration

### Task 1.2: Email Service Setup
**Estimativa:** 1h
- [ ] Configurar Resend
- [ ] Template: verificação de email
- [ ] Template: magic link
- [ ] Template: reset de senha

## Fase 2: Backend - Serviços (1 dia)

### Task 2.1: AuthService
**Estimativa:** 4h
**Dependências:** 1.1, 1.2
- [ ] register()
- [ ] login()
- [ ] verifyEmail()
- [ ] createMagicLink()
- [ ] verifyMagicLink()
- [ ] refreshToken()
- [ ] logout()
- [ ] Testes

### Task 2.2: Auth Routes
**Estimativa:** 3h
**Dependências:** 2.1
- [ ] Todos os endpoints
- [ ] Schemas Zod
- [ ] Middleware JWT
- [ ] Rate limiting

## Fase 3: Frontend (1 dia)

### Task 3.1: Auth Store
**Estimativa:** 2h
- [ ] Zustand store
- [ ] Persistência
- [ ] Interceptor para token

### Task 3.2: Páginas Auth
**Estimativa:** 4h
**Dependências:** 3.1
- [ ] /login
- [ ] /register
- [ ] /forgot-password
- [ ] /reset-password
- [ ] /auth/verify (magic link)
```

---

## Capítulo 7: Spec de Workspaces

```markdown
# .claude/specs/002-workspaces/requirements.md

# Feature: Workspaces

## Visão Geral
Workspaces são espaços de trabalho isolados onde equipes podem
colaborar em tarefas. Cada workspace tem seus próprios membros,
tarefas e configurações.

## User Stories

### US-001: Criar Workspace
**Como** usuário autenticado
**Quero** criar um novo workspace
**Para que** possa organizar tarefas de um projeto/cliente

**Critérios de Aceitação:**
- [ ] Nome obrigatório (2-100 chars)
- [ ] Descrição opcional
- [ ] Ícone/cor selecionável
- [ ] Criador é automaticamente admin
- [ ] Workspace aparece na sidebar

### US-002: Convidar Membros
**Como** admin de um workspace
**Quero** convidar outras pessoas
**Para que** possam colaborar nas tarefas

**Critérios de Aceitação:**
- [ ] Convite por email
- [ ] Definir role: admin ou member
- [ ] Email de convite enviado
- [ ] Link válido por 7 dias
- [ ] Pode reenviar convite
- [ ] Pode cancelar convite pendente

### US-003: Gerenciar Membros
**Como** admin de um workspace
**Quero** gerenciar membros existentes
**Para que** possa alterar permissões ou remover pessoas

**Critérios de Aceitação:**
- [ ] Ver lista de membros com roles
- [ ] Alterar role de membro
- [ ] Remover membro
- [ ] Não pode remover a si mesmo se for único admin
- [ ] Membro removido perde acesso imediatamente

### US-004: Sair do Workspace
**Como** membro de um workspace
**Quero** sair voluntariamente
**Para que** não veja mais esse workspace

**Critérios de Aceitação:**
- [ ] Um clique para sair
- [ ] Confirmação necessária
- [ ] Admin não pode sair se for único admin
- [ ] Tarefas atribuídas ficam sem assignee

## Requisitos Funcionais

### RF-001 (Must Have)
O sistema DEVE isolar completamente dados entre workspaces.

### RF-002 (Must Have)
O sistema DEVE verificar permissão de workspace em toda operação.

### RF-003 (Must Have)
O sistema DEVE manter pelo menos um admin por workspace.

### RF-004 (Should Have)
O sistema DEVE permitir transferir ownership do workspace.

## Roles e Permissões

| Ação | Admin | Member |
|------|-------|--------|
| Criar tarefas | ✅ | ✅ |
| Editar qualquer tarefa | ✅ | ❌ (só próprias) |
| Deletar tarefas | ✅ | ❌ (só próprias) |
| Convidar membros | ✅ | ❌ |
| Remover membros | ✅ | ❌ |
| Editar workspace | ✅ | ❌ |
| Deletar workspace | ✅ | ❌ |
```

```markdown
# .claude/specs/002-workspaces/design.md

# Design: Workspaces
```

### Diagrama de Entidades

```mermaid
erDiagram
    USER ||--o{ WORKSPACE_MEMBER : "pertence a"
    WORKSPACE ||--o{ WORKSPACE_MEMBER : "tem"
    WORKSPACE ||--o{ WORKSPACE_INVITE : "tem"
    WORKSPACE ||--o{ TASK : "contém"
    WORKSPACE ||--o{ TAG : "define"
    
    USER {
        string id PK
        string name
        string email UK
    }
    
    WORKSPACE {
        string id PK
        string name
        string description
        string icon
        string color
    }
    
    WORKSPACE_MEMBER {
        string id PK
        string workspaceId FK
        string userId FK
        enum role "ADMIN | MEMBER"
    }
    
    WORKSPACE_INVITE {
        string id PK
        string workspaceId FK
        string email
        string token UK
        enum role
        datetime expiresAt
    }
```

## Modelo de Dados

```prisma
model Workspace {
  id          String    @id @default(cuid())
  name        String
  description String?
  icon        String?   // emoji ou URL
  color       String    @default("#6366f1") // hex
  
  members     WorkspaceMember[]
  invites     WorkspaceInvite[]
  tasks       Task[]
  tags        Tag[]
  
  createdAt   DateTime  @default(now())
  updatedAt   DateTime  @updatedAt
  
  @@index([name])
}

model WorkspaceMember {
  id          String    @id @default(cuid())
  workspaceId String
  workspace   Workspace @relation(fields: [workspaceId], references: [id], onDelete: Cascade)
  userId      String
  user        User      @relation(fields: [userId], references: [id], onDelete: Cascade)
  
  role        WorkspaceRole @default(MEMBER)
  
  createdAt   DateTime  @default(now())
  updatedAt   DateTime  @updatedAt
  
  @@unique([workspaceId, userId])
  @@index([workspaceId])
  @@index([userId])
}

model WorkspaceInvite {
  id          String    @id @default(cuid())
  workspaceId String
  workspace   Workspace @relation(fields: [workspaceId], references: [id], onDelete: Cascade)
  
  email       String
  role        WorkspaceRole @default(MEMBER)
  token       String    @unique
  
  invitedById String
  
  expiresAt   DateTime
  acceptedAt  DateTime?
  
  createdAt   DateTime  @default(now())
  
  @@index([workspaceId])
  @@index([email])
  @@index([token])
}

enum WorkspaceRole {
  ADMIN
  MEMBER
}
```

## API Endpoints

```yaml
# Workspaces
POST /api/v1/workspaces
  Body: { name, description?, icon?, color? }
  Response 201: Workspace

GET /api/v1/workspaces
  Response 200: Workspace[]

GET /api/v1/workspaces/:id
  Response 200: Workspace (com members)

PATCH /api/v1/workspaces/:id
  Body: { name?, description?, icon?, color? }
  Response 200: Workspace

DELETE /api/v1/workspaces/:id
  Response 204

# Members
GET /api/v1/workspaces/:id/members
  Response 200: WorkspaceMember[]

PATCH /api/v1/workspaces/:id/members/:userId
  Body: { role }
  Response 200: WorkspaceMember

DELETE /api/v1/workspaces/:id/members/:userId
  Response 204

# Invites
POST /api/v1/workspaces/:id/invites
  Body: { email, role? }
  Response 201: WorkspaceInvite

GET /api/v1/workspaces/:id/invites
  Response 200: WorkspaceInvite[]

DELETE /api/v1/workspaces/:id/invites/:inviteId
  Response 204

POST /api/v1/invites/:token/accept
  Response 200: { workspace }
```

## Verificação de Permissões

```typescript
async function checkWorkspaceAccess(
  userId: string,
  workspaceId: string,
  requiredRole?: WorkspaceRole
): Promise<WorkspaceMember> {
  const member = await db.workspaceMember.findUnique({
    where: {
      workspaceId_userId: { workspaceId, userId }
    }
  });
  
  if (!member) {
    throw new ForbiddenError('Não é membro deste workspace');
  }
  
  if (requiredRole === 'ADMIN' && member.role !== 'ADMIN') {
    throw new ForbiddenError('Requer permissão de admin');
  }
  
  return member;
}
```

```markdown
# .claude/specs/002-workspaces/tasks.md

# Tasks: Workspaces

## Fase 1: Backend (1.5 dias)

### Task 1.1: Schema Prisma
**Estimativa:** 1h
- [ ] Model Workspace
- [ ] Model WorkspaceMember
- [ ] Model WorkspaceInvite
- [ ] Enum WorkspaceRole
- [ ] Migration

### Task 1.2: WorkspaceService
**Estimativa:** 4h
**Dependências:** 1.1
- [ ] create()
- [ ] findAllForUser()
- [ ] findById()
- [ ] update()
- [ ] delete()
- [ ] checkAccess()
- [ ] Testes

### Task 1.3: MemberService
**Estimativa:** 3h
**Dependências:** 1.1
- [ ] invite()
- [ ] acceptInvite()
- [ ] updateRole()
- [ ] remove()
- [ ] leave()
- [ ] Testes

### Task 1.4: Workspace Routes
**Estimativa:** 3h
**Dependências:** 1.2, 1.3
- [ ] Todos os endpoints
- [ ] Middleware de workspace access
- [ ] Schemas Zod

## Fase 2: Frontend (1.5 dias)

### Task 2.1: Workspace Store
**Estimativa:** 2h
- [ ] Estado do workspace atual
- [ ] Lista de workspaces do usuário
- [ ] Switching entre workspaces

### Task 2.2: Sidebar com Workspaces
**Estimativa:** 3h
- [ ] Lista de workspaces
- [ ] Indicador do workspace atual
- [ ] Botão criar workspace
- [ ] Menu de contexto

### Task 2.3: Páginas de Workspace
**Estimativa:** 4h
- [ ] Modal criar workspace
- [ ] Página de configurações
- [ ] Gerenciamento de membros
- [ ] Modal de convite
```

---

## Capítulo 8: Spec de Tarefas

```markdown
# .claude/specs/003-tasks/requirements.md

# Feature: Gerenciamento de Tarefas

## Visão Geral
Sistema completo de tarefas com subtasks, tags, due dates,
assignees e atualizações em tempo real.

## User Stories

### US-001: Criar Tarefa
**Como** membro de um workspace
**Quero** criar uma nova tarefa
**Para que** possa registrar trabalho a ser feito

**Critérios de Aceitação:**
- [ ] Título obrigatório (2-500 chars)
- [ ] Descrição opcional (markdown)
- [ ] Due date opcional
- [ ] Assignees opcionais (múltiplos)
- [ ] Tags opcionais (múltiplas)
- [ ] Prioridade: none, low, medium, high, urgent
- [ ] Aparece em tempo real para outros membros

### US-002: Criar Subtask
**Como** membro
**Quero** criar subtasks
**Para que** possa dividir trabalho complexo

**Critérios de Aceitação:**
- [ ] Título obrigatório
- [ ] Máximo 50 subtasks por tarefa
- [ ] Pode marcar como concluída
- [ ] Progresso reflete na tarefa pai

### US-003: Editar Tarefa
**Como** membro
**Quero** editar tarefas
**Para que** possa atualizar informações

**Critérios de Aceitação:**
- [ ] Editar todos os campos
- [ ] Member só edita próprias tarefas
- [ ] Admin edita qualquer tarefa
- [ ] Histórico de alterações mantido

### US-004: Completar Tarefa
**Como** membro
**Quero** marcar tarefa como concluída
**Para que** possa acompanhar progresso

**Critérios de Aceitação:**
- [ ] Toggle com um clique
- [ ] Registra quem e quando completou
- [ ] Pode desfazer
- [ ] Trigger automações configuradas

### US-005: Filtrar e Buscar
**Como** membro
**Quero** filtrar e buscar tarefas
**Para que** possa encontrar rapidamente

**Critérios de Aceitação:**
- [ ] Busca por título/descrição
- [ ] Filtro por status (pendente/concluída)
- [ ] Filtro por assignee
- [ ] Filtro por tag
- [ ] Filtro por due date (hoje, semana, atrasadas)
- [ ] Ordenar por data, prioridade, título

## Requisitos Funcionais

### RF-001 (Must Have)
O sistema DEVE validar permissões antes de qualquer operação.

### RF-002 (Must Have)
O sistema DEVE atualizar em tempo real via WebSocket.

### RF-003 (Must Have)
O sistema DEVE manter histórico de alterações (audit log).

### RF-004 (Should Have)
O sistema DEVE suportar drag-and-drop para reordenar.

## Requisitos Não-Funcionais

### RNF-001: Performance
- Listar tarefas: < 500ms (até 1000 tarefas)
- Criar tarefa: < 300ms
- Atualização real-time: < 200ms latência
```

```markdown
# .claude/specs/003-tasks/design.md

# Design: Tarefas
```

### Diagrama de Estados da Tarefa

```mermaid
stateDiagram-v2
    [*] --> TODO: Criar Tarefa
    
    TODO --> IN_PROGRESS: Iniciar
    TODO --> DONE: Completar
    TODO --> ARCHIVED: Arquivar
    
    IN_PROGRESS --> TODO: Pausar
    IN_PROGRESS --> DONE: Completar
    IN_PROGRESS --> ARCHIVED: Arquivar
    
    DONE --> TODO: Reabrir
    DONE --> ARCHIVED: Arquivar
    
    ARCHIVED --> TODO: Restaurar
    
    DONE --> [*]
    ARCHIVED --> [*]
```

## Modelo de Dados

```prisma
model Task {
  id          String    @id @default(cuid())
  workspaceId String
  workspace   Workspace @relation(fields: [workspaceId], references: [id], onDelete: Cascade)
  
  title       String
  description String?   // Markdown
  priority    TaskPriority @default(NONE)
  status      TaskStatus @default(TODO)
  
  dueDate     DateTime?
  completedAt DateTime?
  completedBy String?
  
  position    Int       @default(0)  // Para ordenação
  
  // Relações
  createdById String
  createdBy   User      @relation("TaskCreator", fields: [createdById], references: [id])
  
  assignees   TaskAssignee[]
  tags        TaskTag[]
  subtasks    Subtask[]
  activities  TaskActivity[]
  
  parentId    String?   // Para subtasks aninhadas (futuro)
  
  createdAt   DateTime  @default(now())
  updatedAt   DateTime  @updatedAt
  
  @@index([workspaceId, status])
  @@index([workspaceId, dueDate])
  @@index([createdById])
}

model Subtask {
  id          String    @id @default(cuid())
  taskId      String
  task        Task      @relation(fields: [taskId], references: [id], onDelete: Cascade)
  
  title       String
  completed   Boolean   @default(false)
  completedAt DateTime?
  position    Int       @default(0)
  
  createdAt   DateTime  @default(now())
  updatedAt   DateTime  @updatedAt
  
  @@index([taskId])
}

model TaskAssignee {
  id          String    @id @default(cuid())
  taskId      String
  task        Task      @relation(fields: [taskId], references: [id], onDelete: Cascade)
  userId      String
  user        User      @relation(fields: [userId], references: [id], onDelete: Cascade)
  
  assignedAt  DateTime  @default(now())
  
  @@unique([taskId, userId])
  @@index([taskId])
  @@index([userId])
}

model Tag {
  id          String    @id @default(cuid())
  workspaceId String
  workspace   Workspace @relation(fields: [workspaceId], references: [id], onDelete: Cascade)
  
  name        String
  color       String    @default("#6b7280")
  
  tasks       TaskTag[]
  
  createdAt   DateTime  @default(now())
  
  @@unique([workspaceId, name])
  @@index([workspaceId])
}

model TaskTag {
  id          String    @id @default(cuid())
  taskId      String
  task        Task      @relation(fields: [taskId], references: [id], onDelete: Cascade)
  tagId       String
  tag         Tag       @relation(fields: [tagId], references: [id], onDelete: Cascade)
  
  @@unique([taskId, tagId])
}

model TaskActivity {
  id          String    @id @default(cuid())
  taskId      String
  task        Task      @relation(fields: [taskId], references: [id], onDelete: Cascade)
  userId      String
  user        User      @relation(fields: [userId], references: [id])
  
  action      String    // created, updated, completed, assigned, etc
  field       String?   // campo alterado
  oldValue    String?
  newValue    String?
  
  createdAt   DateTime  @default(now())
  
  @@index([taskId, createdAt])
}

enum TaskPriority {
  NONE
  LOW
  MEDIUM
  HIGH
  URGENT
}

enum TaskStatus {
  TODO
  IN_PROGRESS
  DONE
  ARCHIVED
}
```

## API Endpoints

```yaml
# Tasks
POST /api/v1/workspaces/:workspaceId/tasks
  Body: { title, description?, priority?, dueDate?, assigneeIds?, tagIds? }
  Response 201: Task

GET /api/v1/workspaces/:workspaceId/tasks
  Query: { status?, assigneeId?, tagId?, dueBefore?, dueAfter?, search?, sort?, page?, limit? }
  Response 200: { data: Task[], pagination }

GET /api/v1/workspaces/:workspaceId/tasks/:taskId
  Response 200: Task (com subtasks, activities)

PATCH /api/v1/workspaces/:workspaceId/tasks/:taskId
  Body: { title?, description?, priority?, status?, dueDate?, assigneeIds?, tagIds?, position? }
  Response 200: Task

DELETE /api/v1/workspaces/:workspaceId/tasks/:taskId
  Response 204

# Subtasks
POST /api/v1/workspaces/:workspaceId/tasks/:taskId/subtasks
  Body: { title }
  Response 201: Subtask

PATCH /api/v1/workspaces/:workspaceId/tasks/:taskId/subtasks/:subtaskId
  Body: { title?, completed?, position? }
  Response 200: Subtask

DELETE /api/v1/workspaces/:workspaceId/tasks/:taskId/subtasks/:subtaskId
  Response 204

# Tags
POST /api/v1/workspaces/:workspaceId/tags
  Body: { name, color? }
  Response 201: Tag

GET /api/v1/workspaces/:workspaceId/tags
  Response 200: Tag[]

PATCH /api/v1/workspaces/:workspaceId/tags/:tagId
  Body: { name?, color? }
  Response 200: Tag

DELETE /api/v1/workspaces/:workspaceId/tags/:tagId
  Response 204
```

## Real-time Events

```typescript
// Eventos emitidos via Socket.io
interface TaskEvents {
  'task:created': { task: Task };
  'task:updated': { task: Task; changes: Partial<Task> };
  'task:deleted': { taskId: string };
  'subtask:created': { taskId: string; subtask: Subtask };
  'subtask:updated': { taskId: string; subtask: Subtask };
  'subtask:deleted': { taskId: string; subtaskId: string };
}

// Room = workspace:${workspaceId}
// Todos os membros do workspace recebem eventos
```

## Activity Logging

```typescript
async function logActivity(
  taskId: string,
  userId: string,
  action: string,
  field?: string,
  oldValue?: any,
  newValue?: any
) {
  await db.taskActivity.create({
    data: {
      taskId,
      userId,
      action,
      field,
      oldValue: oldValue ? JSON.stringify(oldValue) : null,
      newValue: newValue ? JSON.stringify(newValue) : null
    }
  });
}

// Exemplos de uso:
// logActivity(taskId, userId, 'created')
// logActivity(taskId, userId, 'updated', 'title', 'Tarefa antiga', 'Tarefa nova')
// logActivity(taskId, userId, 'completed')
// logActivity(taskId, userId, 'assigned', null, null, 'user_123')
```

```markdown
# .claude/specs/003-tasks/tasks.md

# Tasks: Gerenciamento de Tarefas

## Fase 1: Backend - Models (0.5 dia)

### Task 1.1: Schema Prisma
**Estimativa:** 2h
- [ ] Model Task
- [ ] Model Subtask
- [ ] Model Tag
- [ ] Model TaskTag
- [ ] Model TaskAssignee
- [ ] Model TaskActivity
- [ ] Enums
- [ ] Migration

## Fase 2: Backend - Serviços (2 dias)

### Task 2.1: TaskService
**Estimativa:** 5h
**Dependências:** 1.1
- [ ] create()
- [ ] findAll() com filtros e paginação
- [ ] findById()
- [ ] update()
- [ ] delete()
- [ ] updateStatus()
- [ ] reorder()
- [ ] Testes

### Task 2.2: SubtaskService
**Estimativa:** 2h
**Dependências:** 1.1
- [ ] create()
- [ ] update()
- [ ] delete()
- [ ] toggleComplete()
- [ ] Testes

### Task 2.3: TagService
**Estimativa:** 1.5h
**Dependências:** 1.1
- [ ] create()
- [ ] findAllByWorkspace()
- [ ] update()
- [ ] delete()
- [ ] Testes

### Task 2.4: ActivityService
**Estimativa:** 1.5h
**Dependências:** 1.1
- [ ] log()
- [ ] findByTask()
- [ ] Testes

### Task 2.5: Task Routes
**Estimativa:** 3h
**Dependências:** 2.1, 2.2, 2.3, 2.4
- [ ] Todos os endpoints
- [ ] Validação de permissões
- [ ] Schemas Zod

## Fase 3: Real-time (0.5 dia)

### Task 3.1: Socket.io Setup
**Estimativa:** 2h
- [ ] Configurar servidor Socket.io
- [ ] Middleware de autenticação
- [ ] Rooms por workspace

### Task 3.2: Event Emitters
**Estimativa:** 2h
**Dependências:** 3.1
- [ ] Emitir eventos em create/update/delete
- [ ] Integrar com TaskService
- [ ] Testes

## Fase 4: Frontend (2 dias)

### Task 4.1: Task Store
**Estimativa:** 2h
- [ ] React Query hooks
- [ ] Optimistic updates
- [ ] Socket.io listeners

### Task 4.2: Task List Component
**Estimativa:** 4h
**Dependências:** 4.1
- [ ] Lista de tarefas
- [ ] Filtros e busca
- [ ] Loading states
- [ ] Empty state

### Task 4.3: Task Form
**Estimativa:** 3h
- [ ] Criar/editar tarefa
- [ ] Seletor de assignees
- [ ] Seletor de tags
- [ ] Date picker

### Task 4.4: Task Detail
**Estimativa:** 4h
- [ ] Visualização completa
- [ ] Subtasks
- [ ] Activity log
- [ ] Edição inline

### Task 4.5: Drag and Drop
**Estimativa:** 3h
**Dependências:** 4.2
- [ ] Reordenar tarefas
- [ ] @dnd-kit integration
```

---

## Capítulo 9: Spec de Automações

```markdown
# .claude/specs/004-automations/requirements.md

# Feature: Automações

## Visão Geral
Permitir que usuários configurem automações simples do tipo
"quando X acontecer, faça Y" para reduzir trabalho manual.

## User Stories

### US-001: Criar Automação
**Como** admin de workspace
**Quero** criar uma automação
**Para que** ações repetitivas sejam automatizadas

**Critérios de Aceitação:**
- [ ] Nome obrigatório
- [ ] Selecionar trigger (quando)
- [ ] Selecionar action (então)
- [ ] Pode ativar/desativar
- [ ] Máximo 10 automações por workspace

### US-002: Triggers Disponíveis
- Quando tarefa for criada
- Quando tarefa for completada
- Quando tarefa for atribuída a alguém
- Quando due date passar (tarefa atrasada)
- Quando tag for adicionada

### US-003: Actions Disponíveis
- Criar nova tarefa
- Atribuir tarefa a alguém
- Adicionar tag
- Enviar notificação
- Mover para status

### US-004: Ver Histórico
**Como** admin
**Quero** ver histórico de execuções
**Para que** possa debugar problemas

**Critérios de Aceitação:**
- [ ] Lista de execuções recentes
- [ ] Status: success, failed
- [ ] Detalhes do erro se falhou
- [ ] Timestamp

## Requisitos Funcionais

### RF-001 (Must Have)
O sistema DEVE executar automações de forma assíncrona (queue).

### RF-002 (Must Have)
O sistema DEVE evitar loops infinitos (automação triggera outra).

### RF-003 (Should Have)
O sistema DEVE permitir condições nas automações (se tag = X).

## Exemplo de Automação

```

Nome: "Auto-assign bugs"
Trigger: Quando tarefa for criada
Condição: Tag contém "bug"
Action: Atribuir a "<dev@empresa.com>"

```
```

```markdown
# .claude/specs/004-automations/design.md

# Design: Automações
```

### Fluxo de Execução

```mermaid
flowchart LR
    subgraph TRIGGER["🎯 Trigger"]
        EVT[Evento<br/>task:created]
    end
    
    subgraph QUEUE["📬 Queue"]
        BQ[BullMQ]
    end
    
    subgraph WORKER["⚙️ Worker"]
        PROC[Processar]
        COND{Condições?}
        EXEC[Executar<br/>Action]
        LOG[Registrar<br/>Execução]
    end
    
    EVT --> BQ
    BQ --> PROC
    PROC --> COND
    COND -->|Sim| EXEC
    COND -->|Não| LOG
    EXEC --> LOG
    
    style TRIGGER fill:#3b82f6,color:#fff
    style QUEUE fill:#f59e0b,color:#fff
    style WORKER fill:#10b981,color:#fff
```

## Modelo de Dados

```prisma
model Automation {
  id          String    @id @default(cuid())
  workspaceId String
  workspace   Workspace @relation(fields: [workspaceId], references: [id], onDelete: Cascade)
  
  name        String
  description String?
  enabled     Boolean   @default(true)
  
  trigger     Json      // { type: "task_created", conditions?: {...} }
  action      Json      // { type: "assign_task", params: { userId: "..." } }
  
  executions  AutomationExecution[]
  
  createdById String
  createdAt   DateTime  @default(now())
  updatedAt   DateTime  @updatedAt
  
  @@index([workspaceId, enabled])
}

model AutomationExecution {
  id           String    @id @default(cuid())
  automationId String
  automation   Automation @relation(fields: [automationId], references: [id], onDelete: Cascade)
  
  triggeredBy  String    // taskId que disparou
  status       ExecutionStatus
  error        String?
  result       Json?
  
  startedAt    DateTime  @default(now())
  completedAt  DateTime?
  
  @@index([automationId, startedAt])
}

enum ExecutionStatus {
  PENDING
  RUNNING
  SUCCESS
  FAILED
}
```

## Trigger Types

```typescript
type TriggerType = 
  | 'task_created'
  | 'task_completed'
  | 'task_assigned'
  | 'task_overdue'
  | 'tag_added';

interface TriggerConfig {
  type: TriggerType;
  conditions?: {
    tagIds?: string[];      // Se tiver alguma dessas tags
    assigneeIds?: string[]; // Se atribuído a algum desses
    priority?: TaskPriority[];
  };
}
```

## Action Types

```typescript
type ActionType =
  | 'create_task'
  | 'assign_task'
  | 'add_tag'
  | 'send_notification'
  | 'change_status';

interface ActionConfig {
  type: ActionType;
  params: {
    // Para create_task
    title?: string;
    assigneeIds?: string[];
    
    // Para assign_task
    userId?: string;
    
    // Para add_tag
    tagId?: string;
    
    // Para send_notification
    message?: string;
    
    // Para change_status
    status?: TaskStatus;
  };
}
```

## Prevenção de Loops

```typescript
// Contexto passado para cada execução
interface AutomationContext {
  depth: number;        // Quantas automações já executaram
  sourceTaskId: string; // Tarefa original que triggou
  executedAutomations: string[]; // IDs já executados
}

const MAX_DEPTH = 3;
const MAX_EXECUTIONS_PER_TRIGGER = 5;

function canExecute(context: AutomationContext, automationId: string): boolean {
  if (context.depth >= MAX_DEPTH) return false;
  if (context.executedAutomations.length >= MAX_EXECUTIONS_PER_TRIGGER) return false;
  if (context.executedAutomations.includes(automationId)) return false;
  return true;
}
```

```markdown
# .claude/specs/004-automations/tasks.md

# Tasks: Automações

## Fase 1: Backend (1.5 dias)

### Task 1.1: Schema Prisma
**Estimativa:** 1h
- [ ] Model Automation
- [ ] Model AutomationExecution
- [ ] Enums
- [ ] Migration

### Task 1.2: AutomationService
**Estimativa:** 4h
**Dependências:** 1.1
- [ ] create()
- [ ] findByWorkspace()
- [ ] update()
- [ ] delete()
- [ ] toggle()
- [ ] Testes

### Task 1.3: AutomationEngine
**Estimativa:** 5h
**Dependências:** 1.1
- [ ] findMatchingAutomations()
- [ ] evaluateConditions()
- [ ] executeAction()
- [ ] Loop prevention
- [ ] Testes

### Task 1.4: Queue Setup
**Estimativa:** 2h
- [ ] BullMQ configuration
- [ ] Worker para automations
- [ ] Retry logic

### Task 1.5: Event Integration
**Estimativa:** 2h
**Dependências:** 1.3, 1.4
- [ ] Hook em TaskService
- [ ] Enfileirar automations
- [ ] Testes de integração

## Fase 2: Frontend (1 dia)

### Task 2.1: Automation Form
**Estimativa:** 3h
- [ ] Seletor de trigger
- [ ] Seletor de action
- [ ] Configuração de parâmetros
- [ ] Preview

### Task 2.2: Automation List
**Estimativa:** 2h
- [ ] Lista com toggle on/off
- [ ] Status de última execução
- [ ] Editar/deletar

### Task 2.3: Execution History
**Estimativa:** 2h
- [ ] Lista de execuções
- [ ] Detalhes de erro
- [ ] Filtros
```

---

## Capítulo 10: Spec de Notificações

```markdown
# .claude/specs/005-notifications/requirements.md

# Feature: Notificações

## Visão Geral
Sistema de notificações em tempo real para manter usuários
informados sobre atividades relevantes.

## User Stories

### US-001: Receber Notificações In-App
**Como** usuário
**Quero** ver notificações na aplicação
**Para que** saiba de atualizações importantes

**Critérios de Aceitação:**
- [ ] Badge com contador no header
- [ ] Dropdown com lista de notificações
- [ ] Marcar como lida com um clique
- [ ] Marcar todas como lidas
- [ ] Notificação aparece em tempo real

### US-002: Configurar Preferências
**Como** usuário
**Quero** configurar quais notificações receber
**Para que** não seja sobrecarregado

**Critérios de Aceitação:**
- [ ] Toggle por tipo de notificação
- [ ] Configuração por workspace
- [ ] Opção de receber por email

## Tipos de Notificação

| Tipo | Descrição |
|------|-----------|
| task_assigned | Tarefa atribuída a você |
| task_completed | Tarefa que você criou foi completada |
| task_due_soon | Sua tarefa vence em 24h |
| task_overdue | Sua tarefa está atrasada |
| mention | Você foi mencionado |
| workspace_invite | Convite para workspace |
| automation_executed | Automação executada (se habilitado) |
```

```markdown
# .claude/specs/005-notifications/design.md

# Design: Notificações
```

### Fluxo de Notificações

```mermaid
flowchart TB
    subgraph SOURCES["📤 Fontes"]
        T[Tarefas]
        A[Automações]
        W[Workspaces]
    end
    
    subgraph SERVICE["🔔 NotificationService"]
        CREATE[Criar<br/>Notificação]
        CHECK[Verificar<br/>Preferências]
        SAVE[(Salvar<br/>no DB)]
    end
    
    subgraph DELIVERY["📬 Entrega"]
        WS[Socket.io<br/>Real-time]
        EMAIL[Email<br/>Resend]
    end
    
    subgraph CLIENT["👤 Cliente"]
        BADGE[Badge<br/>Contador]
        LIST[Lista de<br/>Notificações]
    end
    
    T --> CREATE
    A --> CREATE
    W --> CREATE
    
    CREATE --> CHECK
    CHECK --> SAVE
    SAVE --> WS
    SAVE -.->|se habilitado| EMAIL
    
    WS --> BADGE
    WS --> LIST
    
    style SOURCES fill:#3b82f6,color:#fff
    style SERVICE fill:#8b5cf6,color:#fff
    style DELIVERY fill:#f59e0b,color:#fff
    style CLIENT fill:#10b981,color:#fff
```

## Modelo de Dados

```prisma
model Notification {
  id          String    @id @default(cuid())
  userId      String
  user        User      @relation(fields: [userId], references: [id], onDelete: Cascade)
  
  type        NotificationType
  title       String
  message     String?
  
  // Contexto
  workspaceId String?
  taskId      String?
  actorId     String?   // Quem causou a notificação
  
  read        Boolean   @default(false)
  readAt      DateTime?
  
  createdAt   DateTime  @default(now())
  
  @@index([userId, read, createdAt])
  @@index([userId, createdAt])
}

model NotificationPreference {
  id          String    @id @default(cuid())
  userId      String
  user        User      @relation(fields: [userId], references: [id], onDelete: Cascade)
  
  type        NotificationType
  inApp       Boolean   @default(true)
  email       Boolean   @default(false)
  
  @@unique([userId, type])
}

enum NotificationType {
  TASK_ASSIGNED
  TASK_COMPLETED
  TASK_DUE_SOON
  TASK_OVERDUE
  MENTION
  WORKSPACE_INVITE
  AUTOMATION_EXECUTED
}
```

## API Endpoints

```yaml
GET /api/v1/notifications
  Query: { unreadOnly?, limit?, cursor? }
  Response 200: { data: Notification[], nextCursor? }

PATCH /api/v1/notifications/:id/read
  Response 200: Notification

POST /api/v1/notifications/read-all
  Response 204

GET /api/v1/notifications/preferences
  Response 200: NotificationPreference[]

PATCH /api/v1/notifications/preferences/:type
  Body: { inApp?, email? }
  Response 200: NotificationPreference
```

## Real-time

```typescript
// Evento enviado para user específico
interface NotificationEvent {
  type: 'notification:new';
  payload: Notification;
}

// Room = user:${userId}
```

---

# PARTE III: WORKFLOW E TÉCNICAS AVANÇADAS

---

## Capítulo 11: Comandos Slash Customizados

```markdown
# .claude/commands/spec-new.md

Crie uma nova especificação para a feature "$ARGUMENTS".

1. Criar diretório `.claude/specs/XXX-$ARGUMENTS/`
2. Criar template de requirements.md
3. Criar placeholder para design.md
4. Criar placeholder para tasks.md
5. Criar .status com "requirements:draft"

NÃO preencha conteúdos. Apenas estrutura.
```

```markdown
# .claude/commands/spec-requirements.md

Ajude a preencher requisitos da spec atual.

1. Encontre spec com status "requirements:draft"
2. Faça perguntas ao usuário:
   - Qual problema resolve?
   - Quem são os usuários?
   - Quais fluxos principais?
   - Quais casos de erro?
   - Requisitos de performance?
3. Preencha requirements.md
4. Apresente para revisão
5. Após aprovação, atualize status para "requirements:approved"

NÃO avance sem aprovação explícita.
```

```markdown
# .claude/commands/spec-design.md

Crie documento de design baseado nos requisitos aprovados.

1. Leia spec com status "requirements:approved"
2. Leia tech-stack.md
3. Crie design incluindo:
   - Modelo de dados (Prisma)
   - API endpoints
   - Fluxos de dados
   - Decisões técnicas justificadas
   - Real-time events (se aplicável)
4. Apresente para revisão
5. Após aprovação, atualize status para "design:approved"
```

```markdown
# .claude/commands/spec-tasks.md

Decomponha design em tasks implementáveis.

1. Leia spec com status "design:approved"
2. Crie tasks seguindo regras:
   - Cada task: 2-4 horas
   - Dependências claras
   - Testável independentemente
3. Organize em fases
4. Apresente para revisão
5. Após aprovação, atualize status para "tasks:approved"
```

```markdown
# .claude/commands/spec-implement.md

Implemente a task especificada.

Uso: /spec-implement [task-id]

1. Encontre task com ID "$ARGUMENTS"
2. Verifique dependências completas
3. Implemente seguindo:
   - Convenções do projeto
   - Design aprovado
   - Escreva testes junto
4. Marque task como [x]
5. Sugira próxima task

NUNCA modifique specs sem permissão.
```

---

## Capítulo 12: Subagentes Especializados

```mermaid
flowchart LR
    subgraph AGENTS["🤖 Subagentes"]
        ARC["🏗️ Architect<br/>Decisões técnicas"]
        IMP["💻 Implementer<br/>Código limpo"]
        REV["🔍 Reviewer<br/>Qualidade"]
    end
    
    REQ[Requirements] --> ARC
    ARC --> DES[Design]
    DES --> IMP
    IMP --> COD[Code]
    COD --> REV
    REV -->|feedback| IMP
    
    style ARC fill:#3b82f6,color:#fff
    style IMP fill:#10b981,color:#fff
    style REV fill:#f59e0b,color:#fff
```

```markdown
# .claude/agents/architect.md

# Subagente: Architect

## Papel
Arquiteto de software focado em decisões técnicas e trade-offs.

## Responsabilidades
- Decisões arquiteturais
- Interfaces entre componentes
- Escolha de tecnologias
- Identificação de riscos

## Regras
1. SEMPRE justifique decisões
2. SEMPRE considere trade-offs
3. NUNCA escreva código de implementação
4. PRIORIZE simplicidade
```

```markdown
# .claude/agents/implementer.md

# Subagente: Implementer

## Papel
Desenvolvedor focado em implementação limpa e testável.

## Regras
1. SIGA a spec exatamente
2. ESCREVA testes junto com código
3. USE tipos TypeScript estritos
4. PARE se encontrar ambiguidade
```

```markdown
# .claude/agents/reviewer.md

# Subagente: Reviewer

## Papel
Code reviewer focado em qualidade e segurança.

## Checklist
- [ ] Segue convenções
- [ ] Tipos corretos
- [ ] Error handling adequado
- [ ] Testes cobrem casos principais
- [ ] Sem vulnerabilidades óbvias
- [ ] Performance aceitável
```

---

## Capítulo 13: Conclusão

### Recapitulação

1. **SDD resolve o problema de contexto:** Agentes de IA precisam de especificações explícitas.

2. **Quatro pilares:** Requirements → Design → Tasks → Implementation, com gates de aprovação.

3. **Specs efetivas:** Seja específico, use exemplos, antecipe perguntas, defina escopo negativo.

4. **Estrutura organizada:** CLAUDE.md conciso, steering files, specs por feature.

5. **Workflow prático:** Comandos slash, subagentes, integração com Git.

### O Mindset SDD

> "Investir tempo pensando antes de fazer economiza tempo total."

Quando você trabalha com agentes de IA, mover rápido sem direção é apenas uma forma mais rápida de criar dívida técnica.

### Próximos Passos

1. **Comece pequeno:** Uma feature, spec completa
2. **Itere:** Adapte templates ao seu estilo
3. **Compartilhe:** SDD funciona melhor em equipe
4. **Contribua:** Melhore templates, documente aprendizados

---

## Apêndice A: Templates Prontos

### Template: requirements.md

```markdown
# Feature: [Nome]

## Visão Geral
[Descrição em 1 parágrafo]

## User Stories

### US-001: [Título]
**Como** [usuário]
**Quero** [ação]
**Para que** [benefício]

**Critérios de Aceitação:**
- [ ] [Critério 1]
- [ ] [Critério 2]

## Requisitos Funcionais

### RF-001 (Must Have)
[Requisito]

## Requisitos Não-Funcionais

### RNF-001: Performance
[Métricas]

## Fora do Escopo
- [Item 1]

## Glossário
- **Termo:** Definição
```

### Template: design.md

```markdown
# Design: [Feature]

> **Requisitos:** @requirements.md
> **Status:** [Draft | Em Revisão | Aprovado]

## 1. Modelo de Dados
[Prisma schema]

## 2. API Endpoints
[Endpoints YAML]

## 3. Fluxos
[Diagramas Mermaid]

## 4. Real-time Events
[Se aplicável]

## 5. Decisões Técnicas
[Justificativas]
```

### Template: tasks.md

```markdown
# Tasks: [Feature]

> **Estimativa:** [X dias]

## Fase 1: [Nome] ([tempo])

### Task 1.1: [Título]
**Prioridade:** P0
**Estimativa:** [tempo]
**Dependências:** [tasks]

- [ ] [Subtask]

**Critérios de Aceite:**
- [Critério]

**Arquivos:**
- `path/to/file.ts`

## Dependências
[Diagrama Mermaid]
```

---

## Apêndice B: Glossário

| Termo | Definição |
|-------|-----------|
| **SDD** | Spec-Driven Development |
| **Gate** | Ponto de decisão entre fases |
| **Steering** | Arquivos de contexto persistente |
| **EARS** | Easy Approach to Requirements Syntax |
| **Workspace** | Espaço de trabalho isolado |
| **Task** | Unidade de trabalho |
| **Subtask** | Subdivisão de uma task |
| **Automation** | Regra "quando X, faça Y" |

---

## Apêndice C: Índice de Specs

```markdown
# .claude/specs/INDEX.md

| ID | Nome | Status |
|----|------|--------|
| 001 | auth | Referência |
| 002 | workspaces | Referência |
| 003 | tasks | Referência |
| 004 | automations | Referência |
| 005 | notifications | Referência |
```

---

## Apêndice D: Tipos de Diagramas Mermaid

Este ebook utiliza diagramas Mermaid para visualização. Aqui estão os tipos usados:

### Flowchart (Fluxogramas)

```mermaid
flowchart LR
    A[Início] --> B{Decisão}
    B -->|Sim| C[Ação 1]
    B -->|Não| D[Ação 2]
```

### Sequence Diagram (Sequência)

```mermaid
sequenceDiagram
    participant A as Cliente
    participant B as Servidor
    A->>B: Request
    B->>A: Response
```

### State Diagram (Estados)

```mermaid
stateDiagram-v2
    [*] --> Estado1
    Estado1 --> Estado2
    Estado2 --> [*]
```

### Entity Relationship (ER)

```mermaid
erDiagram
    USER ||--o{ TASK : creates
    TASK {
        string id
        string title
    }
```

---

*Este ebook foi criado para desenvolvedores experientes adotarem Spec-Driven Development com Claude Code.*

*O TaskFlow Pro é um projeto exemplo para ilustrar conceitos. As specs são funcionais e adaptáveis para projetos reais.*

*Diagramas renderizados com Mermaid - compatível com GitHub, VS Code e principais editores Markdown.*
