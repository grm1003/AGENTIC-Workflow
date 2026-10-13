# 🚀 AI-SDLC: Autonomous Agentic Software Development Lifecycle

Este repositório encapsula o framework e o fluxo operacional de **AI-SDLC (Software Development Life Cycle Orientado a Agentes)** utilizado pelo Gabriel para desenvolvimento Backend (Java & Spring Boot) e Frontend.

O objetivo deste fluxo é garantir **velocidade máxima de entrega com zero sobre-engenharia**, mantendo controle humano estrito através de contratos e especificações rastreáveis em arquivos.

---

## 🧠 Filosofia Central: O Modo Ponytail

Operamos sob o princípio do **Lazy Senior Dev**: *"a melhor linha de código é a que não precisa ser escrita"*.

Antes de escrever qualquer código, o agente percorre a **Ponytail Ladder**:
1. **YAGNI:** Isso realmente precisa existir? Se for especulativo, é descartado.
2. **Reuso:** Já existe utilitário ou lógica pronta no projeto?
3. **Java Stdlib:** A biblioteca padrão resolve (`java.time`, `Stream`, `Optional`)?
4. **Spring Boot Native:** O ecossistema nativo resolve (`@ControllerAdvice`, Bean Validation, Spring Data)?
5. **Libs existentes:** Dependências já instaladas resolvem sem adicionar novas?
6. **Simplicidade:** Pode ser feito em poucas linhas concisas?
7. **Mínimo Viável:** Apenas o estritamente necessário para cumprir o contrato.

---

## 🔄 Máquina de Estados do Ciclo de Vida (Fases)

O ciclo é rigorosamente orquestrado em **4 fases**, usando artefatos versionados no repositório:

```
[Demanda Inicial]
       │
       ▼
┌────────────────────────────────────────┐
│  Fase 1: Grill-Me-With-Docs            │  Analítico, incisivo e YAGNI.
│  (3 a 5 perguntas cirúrgicas + REST)   │
└──────────────────┬─────────────────────┘
                   │ Respostas fornecidas
                   ▼
┌────────────────────────────────────────┐
│  Fase 2: To-Spec & To-Tickets          │  Gera 01-spec.md e 02-tickets.md.
│  🛑 GATE DE APROVAÇÃO HUMANO           │  A IA PARA OBRIGATORIAMENTE.
└──────────────────┬─────────────────────┘
                   │ Gabriel aprova ("Go", "Aprovado")
                   ▼
┌────────────────────────────────────────┐
│  Fase 3: Modo Turbo (Branch Autônoma)  │  Cria branch feature/<slug>, implementa
│  (Execução contínua + mvn test)        │  seguindo AGENT_JAVA.MD e valida testes.
└──────────────────┬─────────────────────┘
                   │ Testes e implementação OK
                   ▼
┌────────────────────────────────────────┐
│  Fase 4: Code-Review & Pull Request    │  Auditoria Ponytail + PMD/Checkstyle,
│  (03-review.md, git push e gh pr)      │  cria commit semântico e abre PR.
└────────────────────────────────────────┘
```

### 1. Fase 1: Grill-Me-With-Docs (Incisivo & Analítico)
* **Gatilho:** Nova demanda recebida e não há especificação no workspace.
* **Ação:** Questiona escopo supérfluo, propõe contrato REST preliminar e faz 3 a 5 perguntas essenciais para cravar as regras de negócio.

### 2. Fase 2: To-Spec & To-Tickets (Especificação e Fatiamento)
* **Gatilho:** Respostas da Fase 1 alinhadas.
* **Ação:** Cria os arquivos `01-spec.md` (especificação de negócio/técnica) e `02-tickets.md` (micro-tarefas atômicas e testáveis).
* 🛑 **GATE OBRIGATÓRIO:** A IA **não toca em código nem cria branch** até que você valide e dê o aval ("Go", "Aprovado", "Pode tocar").

### 3. Fase 3: Modo Turbo (Execução Autônoma)
* **Gatilho:** Aprovação explícita em `02-tickets.md`.
* **Ação:**
  1. Criação automática da branch: `git checkout -b feature/<slug-da-demanda>` a partir da `master`.
  2. Implementação ponta a ponta respeitando [AGENT_JAVA.MD](.agents/AGENT_JAVA.MD) (Services < 150 linhas, `@Builder`, zero try-catch desnecessário, logging cirúrgico na entrada e integração).
  3. Validação contínua com suíte de testes (`mvn test`).

### 4. Fase 4: Review, Push e Pull Request
* **Gatilho:** Implementação e validação de testes concluídas.
* **Ação:**
  1. Auditoria Ponytail + PMD/Checkstyle gerando `03-review.md`.
  2. Commit semântico (ex: `feat(domain): implementa novos endpoints`).
  3. Push para a branch remota (`git push -u origin feature/<slug-da-demanda>`).
  4. Abertura do Pull Request via `gh pr create` (ou link direto pré-formatado).

---

## 📂 Estrutura de Arquivos

```text
AGENTIC-Workflow/
├── README.md               # Este documento explicativo
└── .agents/
    ├── GEMINI.md           # Master prompt / Regras centrais do agente & máquina de estados
    ├── AGENT_JAVA.MD       # Diretrizes técnicas Java & Spring Boot (limites, PMD, logs, design)
    └── skills/             # Habilidades modulares do fluxo
        ├── grill-with-docs/    # Questionamento e refinamento analítico
        ├── to-spec/            # Elaboração do 01-spec.md
        ├── to-tickets/         # Fatiamento do 02-tickets.md
        ├── implement/          # Execução autônoma orientada a tickets
        ├── code-review/        # Auditoria em dois eixos (padrões e requisitos)
        ├── writing-for-agents/ # Criação e manutenção de skills e docs
        ├── ponytail/           # Núcleo da filosofia anti-complexidade
        ├── ponytail-review/    # Review caçador de over-engineering
        ├── ponytail-audit/     # Auditoria de complexidade do repositório
        ├── ponytail-debt/      # Rastreamento de débitos marcados (// ponytail:)
        ├── ponytail-gain/      # Scorecard de métricas de economia
        └── ponytail-help/      # Cartão de ajuda dos comandos
```

---

## 🛠️ Como Portar e Restaurar em Outros Ambientes/Projetos

Para usar este mesmo fluxo em qualquer outro projeto ou máquina:

1. **Clonar ou copiar a pasta `.agents`:**
   Copie a pasta `.agents/` inteira para a raiz do seu novo projeto ou workspace.
2. **Definir regras do agente:**
   Se a sua ferramenta/IDE for o **Antigravity / Gemini CLI / Cursor / Claude Code**, certifique-se de que ela leia as instruções de `.agents/GEMINI.md` como system prompt ou rule central.
3. **Validar ferramentas de CLI:**
   - Git configurado com acesso ao GitHub/GitLab.
   - GitHub CLI (`gh`) autenticado para abertura automática de PRs.
   - Java SDK e Maven (`mvn`) disponíveis no `PATH`.
4. **Acionar o fluxo:**
   Basta enviar a demanda crua na conversa; o agente detectará a ausência de `01-spec.md` e iniciará imediatamente na **Fase 1**.

