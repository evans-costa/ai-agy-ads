---
trigger: always_on
description: "Arquitetura Feature-based, estrutura de pastas, regras de isolamento e roteamento tipado com TanStack Router."
---

# Arquitetura e Estrutura Técnica - Frontend Lessons Hub

> **Descrição**: Estrutura modular baseada em funcionalidades (Feature-based), roteamento tipado com TanStack Router e regras de isolamento de código.

---

## 1. Visão Geral da Arquitetura

O frontend é uma SPA estruturada em **React 19 + TypeScript + Vite + Tailwind CSS** sob o padrão **Feature-based**:
- Cada domínio de negócio agrupa seus próprios componentes, hooks, chamadas de API e tipos.
- Componentes genéricos e primitivos do Design System residem isolados em `src/components/ui`.
- O roteamento tipado e controle de acesso são centralizados pelo **TanStack Router**.

---

## 2. Mapa Estrutural de Diretórios (`src/`)

- **`features/<dominio>/`**: Módulos de negócio isolados contendo subpastas `api/` (queries/mutations), `components/` (UI específica), `hooks/` e `types/` (schemas Zod e DTOs):
  - `auth`: Autenticação, login, sessão e tokens JWT.
  - `dashboard`: Métricas, cálculo do índice Double-Loop e listagem principal.
  - `lessons`: Wizard em 3 etapas (AAR, RCA 5 Porquês, PDCA), modal PDCA e visualização.
  - `admin`: Painel administrativo com abas (Usuários, Projetos, Lições, Categorias).
- **`components/`**:
  - `ui/`: Primitivos reutilizáveis do Design System (Button, Input, Modal, Badge, Table).
  - `layout/`: Componentes estruturais de casca (Header, UserDropdown, Container).
- **`routes/`**: Árvore de rotas do TanStack Router (`__root.tsx`, `index.tsx`, `login.tsx`, `dashboard.tsx`, `lessons/new.tsx`, `lessons/$lessonId.tsx`, `admin/`).
- **`shared/`**: Recursos compartilhados globalmente:
  - `lib/`: Instância centralizada do Axios (`axios.ts`), configuração do React Query (`query-client.ts`) e utilitários (`utils.ts`).
  - `hooks/`: Hooks utilitários globais.
  - `types/`: Tipos compartilhados (respostas paginadas, RFC 7807).
- **`assets/`**: Assets gráficos locais (SVGs e PNGs fiéis ao Figma).

---

## 3. Regras de Isolamento e Dependências

1. **Isolamento entre Features**: Uma feature **nunca** deve importar componentes internos de outra feature diretamente.
2. **Promoção de Código Compartilhado**: Se um componente, hook ou tipo precisar ser compartilhado entre features, promova-o para `src/components/ui`, `src/components/layout` ou `src/shared`.
3. **Encapsulamento de API**: Cada feature gerencia suas próprias chamadas HTTP e chaves de cache em seu subdiretório `api/`.
4. **Contratos DTO**: As interfaces em `types/` devem mapear rigorosamente os DTOs do backend expostos em `LessonsHub.Communication`.

---

## 4. Roteamento com TanStack Router

- **Type Safety**: Rotas com parâmetros de URL e search params estritamente tipados.
- **Proteção de Rotas**: Rotas autenticadas (Dashboard, Wizard, Admin) validam sessão no `beforeLoad`, redirecionando usuários anônimos para `/login`.
- **Controle por Papel (RBAC)**: A rota `/admin` valida se o usuário possui papel `Admin`; caso contrário, redireciona com bloqueio de acesso.
