---
trigger: always_on
---

# Lessons Hub - Master Orchestrator (Global)

> **Descrição**: Ponto de entrada e orquestrador unificado de governança, regras de negócio e diretrizes técnicas para todo o ecossistema do Lessons Hub.

---

Este repositório é organizado no formato **monorepo**, contendo a solução de backend em **.NET 10**, o front-end em **React 19 / TypeScript** e a documentação acadêmica e de contexto.

Este arquivo atua como o **mapa central único de instruções** para agentes de IA e desenvolvedores.

---

## 1. Diretrizes Globais, Governança e Negócio

Sempre consulte os documentos centrais abaixo para garantir alinhamento com a visão do produto e as regras de governança:

- [briefing.md](./rules/briefing.md): **Visão Canônica de Negócio**: O que é o Lessons Hub, combate ao "Cemitério de Lições", as 4 metodologias integradas (AAR, RCA 5 Porquês, SECI, PDCA), fórmula do Índice Double-Loop Learning, papéis de acesso (RBAC) e ciclo de vida da lição (`LessonStatus`).
- [git-workflow.md](./rules/git-workflow.md): **Fluxo Git & Controle de Versão**: Estratégia de branches baseada no GitHub Flow, padrão Conventional Commits, autoria de commits e checklist de validação pré-commit.
- [user-preferences.md](./rules/user-preferences.md): **Governança & Preferências do Usuário**: Aprovação prévia obrigatória para alterações de arquivos e política estrita de gestão manual de dependências (npm e NuGet) pelo usuário.
- [agent-guidelines.md](./rules/agent-guidelines.md): **Diretrizes Gerais do Agente**: Ordem de prioridade de regras, checklist pré-implementação e integridade da base de código.

---

## 2. Backend (.NET 10 / C#)

Diretrizes arquiteturais, padrões de código, persistência e testes da solução em `codigo-fonte/backend`:

### 2.1. Diretrizes de Desenvolvimento e CLI
- [agent-guidelines.md (Backend)](../codigo-fonte/backend/.agents/rules/agent-guidelines.md): Diretrizes específicas para C#/.NET, identificação de camadas da Clean Architecture e convenções de nomenclatura.
- [commands.md (Backend)](../codigo-fonte/backend/.agents/rules/commands.md): Comandos CLI (`dotnet build`, testes com xUnit v3, migrações EF Core com PostgreSQL via Docker e formatação com `dotnet format`).

### 2.2. Arquitetura e Estrutura Técnica
- [architecture.md](../codigo-fonte/backend/.agents/rules/architecture.md): Estrutura Clean Architecture da solução, regras de dependência entre camadas (`Exception`, `Domain`, `Communication`, `Application`, `Infrastructure`, `API`) e injeção de dependência (DI).
- [domain-entities.md](../codigo-fonte/backend/.agents/rules/domain-entities.md): Entidades POCO de domínio, identificadores (Guid/int) e Enums de negócio.
- [application-usecases.md](../codigo-fonte/backend/.agents/rules/application-usecases.md): Camada `Application` com o padrão Use Cases, DTOs em `Communication` e validação com `FluentValidation`.
- [database-repository.md](../codigo-fonte/backend/.agents/rules/database-repository.md): Padrões de persistência com EF Core e PostgreSQL na `Infrastructure`, repositórios segregados por intenção e transações com `IUnitOfWork`.
- [error-handling.md (Backend)](../codigo-fonte/backend/.agents/rules/error-handling.md): Tratamento global de exceções via `IExceptionHandler` (`GlobalExceptionHandler`), hierarquia `LessonsHubException`, respostas RFC 7807/9110 (`ProblemDetails`) e internacionalização de erros em `.resx`.
- [testing-tdd.md (Backend)](../codigo-fonte/backend/.agents/rules/testing-tdd.md): Suíte de testes automatizados com xUnit v3, Shouldly, Testcontainers (PostgreSQL) e utilitários em `CommonTestUtilities`.

### 2.3. Plugins Especializados (.NET)
- [dotnet-test](../codigo-fonte/backend/.agents/plugins/dotnet-test/README.md): Suíte de agentes e habilidades para geração, auditoria e execução de testes em .NET.
- [dotnet-diag](../codigo-fonte/backend/.agents/plugins/dotnet-diag/README.md): Diagnóstico, traces e profiling em ecossistema .NET.
- [dotnet11](../codigo-fonte/backend/.agents/plugins/dotnet11/README.md): Práticas e utilitários modernos para JSON e runtime .NET.

---

## 3. Frontend (React 19 / TypeScript)

Diretrizes visuais, regras de design system, gerenciamento de estado e padrões do cliente web em `codigo-fonte/frontend`:

### 3.1. Diretrizes de Desenvolvimento e CLI
- [agent-guidelines.md (Frontend)](../codigo-fonte/frontend/.agents/rules/agent-guidelines.md): Diretrizes de desenvolvimento para React 19/TS, checklist pré-implementação e convenções de nomenclatura.
- [commands.md (Frontend)](../codigo-fonte/frontend/.agents/rules/commands.md): Comandos CLI (desenvolvimento local com Vite, compilação de tipos TypeScript, testes com Vitest/Playwright e formatação).
- [linting-and-standards.md](../codigo-fonte/frontend/.agents/rules/linting-and-standards.md): Padrões rigorosos de qualidade baseados no Ultracite (TypeScript estrito, React 19, acessibilidade, performance, segurança e clean code).

### 3.2. Arquitetura, Design System e Estrutura Técnica
- [architecture.md (Frontend)](../codigo-fonte/frontend/.agents/rules/architecture.md): Estrutura Feature-based (`src/features/<modulo>`), roteamento tipado com TanStack Router, camada compartilhada (`src/components/ui`, `src/shared`) e árvore de diretórios.
- [DESIGN.md](../codigo-fonte/frontend/.agents/rules/DESIGN.md): **Fonte canônica da verdade** para design tokens, paleta de cores, tipografia Inter, espaçamentos, bordas, componentes (botões, inputs, selects, wizard de 3 passos, tabelas, modais) e ícones (assets SVG locais e Lucide React) extraídos do Figma.
- [state-and-data-fetching.md](../codigo-fonte/frontend/.agents/rules/state-and-data-fetching.md): Gerenciamento de estado de servidor com TanStack Query (React Query), cliente Axios com interceptors JWT/refresh, invalidação de cache e autenticação.
- [forms-and-validation.md](../codigo-fonte/frontend/.agents/rules/forms-and-validation.md): Gestão de formulários com React Hook Form integrado a schemas Zod (espelhando os contratos DTO da API) e formulário multi-etapas (Wizard).
- [error-handling.md (Frontend)](../codigo-fonte/frontend/.agents/rules/error-handling.md): Tratamento de respostas de erro RFC 7807/9457 (`ProblemDetails` e `ValidationProblemDetails`), notificações em toasts via Sonner, validação visual em campos e Error Boundaries.
- [testing-tdd.md (Frontend)](../codigo-fonte/frontend/.agents/rules/testing-tdd.md): Suíte de testes automatizados com Vitest, React Testing Library e testes end-to-end (E2E) com Playwright.

---

## 4. Documentação Acadêmica e de Especificação

- Documentação acadêmica e de negócio em [documentos/](../documentos/):
  - `01-Documentação de Contexto.md`
  - `02-Especificação do Projeto.md`
  - `03-Projeto de Interface.md`
  - `04-Plano de Testes de Software.md`
