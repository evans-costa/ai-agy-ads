---
trigger: always_on
description: "Diretrizes essenciais do agente para frontend, convenções de código React 19, TypeScript estrito e integração com Design System."
---

# Diretrizes e Fluxo de Trabalho do Agente - Frontend Lessons Hub

> **Descrição**: Diretrizes de atuação para agentes de IA no desenvolvimento do frontend: hierarquia de regras, checklist pré-implementação e convenções de código (React 19, TypeScript estrito, TanStack Router).

---

## 1. Ordem de Prioridade

Em caso de instruções conflitantes, siga estritamente esta ordem:
1. **Instruções explícitas do usuário** no chat.
2. **`AGENTS.md` raiz** (`.agents/AGENTS.md`).
3. **`AGENTS.md` do frontend** (`codigo-fonte/frontend/.agents/AGENTS.md`).
4. **Design Spec do Figma** em [DESIGN.md](./DESIGN.md) (fonte canônica da verdade visual).
5. **Regras técnicas específicas** na pasta `.agents/rules/`.
6. **Convenções do ecossistema** (React 19, TypeScript estrito, Vite, Tailwind v4).

---

## 2. Checklist Pré-Implementação

Antes de criar ou alterar qualquer arquivo:
1. **Escopo**: Determine se pertence a uma feature (`src/features/<modulo>`), UI compartilhada (`src/components/ui`), rotas (`src/routes`) ou global (`src/shared`).
2. **Regras**: Consulte o documento temático aplicável ([DESIGN.md](./DESIGN.md), [forms-and-validation.md](./forms-and-validation.md), [state-and-data-fetching.md](./state-and-data-fetching.md)).
3. **Contratos**: Confirme sincronia com os DTOs do backend (`LessonsHub.Communication`).
4. **Reutilização**: Verifique primitivos existentes antes de criar novos componentes.
5. **Design System**: Aplique exclusivamente tokens e classes mapeadas em [DESIGN.md](./DESIGN.md).

---

## 3. Convenções de Nomenclatura

- **Componentes React**: PascalCase (ex: `LessonCard.tsx`, `StepIndicator.tsx`).
- **Custom Hooks**: camelCase iniciando com `use` (ex: `useLessonsQuery.ts`, `useAuth.ts`).
- **Arquivos de Rotas**: Padrão TanStack Router (ex: `lessons.index.tsx`, `lessons.$lessonId.tsx`).
- **Tipos e Interfaces**: PascalCase (ex: `Lesson`, `CreateLessonFormData`, `UserRole`).
- **Schemas Zod**: camelCase terminando em `Schema` (ex: `createLessonSchema`, `loginSchema`).

---

## 4. Regras Mandatórias de Execução

- **TypeScript Estrito**: `strict: true`. Proibido `any`; utilize `unknown` com narrowing.
- **Fidelidade Funcional**: Nunca invente rotas, fluxos de autenticação ou regras de negócio sem confirmação prévia com o usuário.
- **Isolamento Modular**: Respeite a estrutura feature-based definida em [architecture.md](./architecture.md).
- **Regras Especializadas**: Respeite a aprovação prévia e gestão manual de pacotes em [user-preferences.md](../../../../.agents/rules/user-preferences.md), a ausência de comentários dispensáveis, escala nativa Tailwind e qualidade estrita em [linting-and-standards.md](./linting-and-standards.md).
