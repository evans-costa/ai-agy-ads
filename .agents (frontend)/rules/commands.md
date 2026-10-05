---
trigger: always_on
description: "Comandos CLI essenciais para desenvolvimento local, typecheck, testes Vitest/Playwright e Ultracite."
---

# Comandos de Execução e CLI - Frontend Lessons Hub

> **Descrição**: Referência rápida de comandos CLI executados a partir da raiz do frontend (`codigo-fonte/frontend`).

---

## 1. Desenvolvimento e Compilação

| Finalidade | Comando | Descrição |
|---|---|---|
| **Servidor Local** | `npm run dev` | Inicia Vite com HMR em `http://localhost:5173` |
| **Checagem Estática** | `npx tsc --noEmit` | Valida integridade de tipos TypeScript |
| **Build de Produção** | `npm run build` | Compilação otimizada para produção |
| **Preview da Build** | `npm run preview` | Pré-visualização local da pasta dist |

---

## 2. Testes Automatizados

| Finalidade | Comando |
|---|---|
| **Vitest (Unitários/UI)** | `npm run test` (execução única) · `npx vitest` (modo watch) · `npx vitest --ui` |
| **Cobertura de Código** | `npx vitest run --coverage` |
| **Arquivo Específico** | `npx vitest run src/features/<caminho>/<arquivo>.test.tsx` |
| **Playwright (E2E)** | `npx playwright test` (headless) · `npx playwright test --ui` (interativo) |
| **Relatório E2E** | `npx playwright show-report` |

---

## 3. Linting e Formatação (Ultracite)

| Finalidade | Atalho npm | Comando Direto |
|---|---|---|
| **Verificar inconsistências** | `npm run lint` | `npx ultracite check` |
| **Formatar e corrigir** | `npm run fix` | `npx ultracite fix` |
| **Diagnóstico de ambiente** | — | `npx ultracite doctor` |
