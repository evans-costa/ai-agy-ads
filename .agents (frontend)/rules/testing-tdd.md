---
trigger: glob
globs:
  - "src/**/*.test.{ts,tsx}"
  - "tests/**/*"
description: "Padrões de testes automatizados com Vitest, React Testing Library e Playwright E2E."
---

# Testes Automatizados e Qualidade de Software - Frontend Lessons Hub

> **Descrição**: Diretrizes para escrita de testes unitários, testes de componentes e testes ponta a ponta (E2E) com Vitest, React Testing Library e Playwright.

---

## 1. Estrutura da Suíte de Testes

O frontend divide seus testes em duas camadas complementares de qualidade:

1. **Testes Unitários e de Componentes**:
   - **Test Runner**: **Vitest** (rápido, nativo com Vite, suporte a TypeScript out-of-the-box).
   - **DOM Simulator**: **jsdom**.
   - **Component Testing**: **React Testing Library** (`@testing-library/react`, `@testing-library/user-event`, `@testing-library/jest-dom`).
2. **Testes End-to-End (E2E)**:
   - **Framework**: **Playwright** (`@playwright/test`).
   - Simulação de navegação real em navegadores (Chromium, Firefox, WebKit), cobrindo jornadas críticas completas do usuário.

---

## 2. Organização dos Arquivos de Teste

- **Testes de Componentes e Hooks**: Co-localizados junto ao código-fonte da respectiva funcionalidade:
  ```
  src/features/lessons/components/
  ├── LessonCard.tsx
  └── LessonCard.test.tsx
  src/features/lessons/hooks/
  ├── useWizardNavigation.ts
  └── useWizardNavigation.test.ts
  ```
- **Utilitários e Providers de Teste**:
  - `src/test/setup.ts`: Configuração global do Vitest, limpeza do DOM (`afterEach(cleanup)`) e extensões de asserção do jest-dom.
  - `src/test/test-utils.tsx`: Custom render encapsulando os providers obrigatórios da aplicação (`QueryClientProvider`, contexto de autenticação e memória do router).
- **Testes de Ponta a Ponta (E2E)**:
  - Centralizados no diretório `tests/e2e/`:
    ```
    tests/e2e/
    ├── auth.spec.ts            # Login, validação de campos e logout
    ├── lesson-wizard.spec.ts   # Fluxo de criação de lição nos 3 passos
    ├── pdca-closure.spec.ts    # Fechamento de ciclo com registro de evidência
    └── admin-panel.spec.ts     # Acesso e navegação nas abas do admin
    ```

---

## 3. Boas Práticas com React Testing Library

- **Foco no Usuário e Acessibilidade**:
  - Priorize queries por acessibilidade: `getByRole('button', { name: /continuar/i })`, `getByRole('textbox', { name: /título/i })`, `getByLabelText(...)`.
  - Evite expressamente seletores frágeis baseados em classes CSS ou IDs internos (`container.querySelector('.btn-blue')`).
- **Simulação Realista de Eventos**:
  - Utilize sempre `@testing-library/user-event` em vez de `fireEvent` para simular cliques, digitação e foco com fidelidade à interação do usuário.
- **Isolamento de Requisições HTTP**:
  - Em testes unitários e de componentes, nunca realize chamadas HTTP reais à API. Utilize **Mock Service Worker (MSW)** ou mocks com `vi.fn()`/`vi.spyOn()`.
- **Cobertura de Cenários de Borda**:
  - Teste sempre o caminho feliz, o estado de carregamento (*loading skeleton/spinner*), erros de validação e respostas de falha da API (ex: erro 400 ou 401).

---

## 4. Diretrizes para Testes E2E (Playwright)

- **Testes Idempotentes**: Cada teste E2E deve ser independente, preparando seu próprio estado ou utilizando credenciais de teste configuradas.
- **Resiliência a Delays de Rede**: Prefira seletores automáticos com auto-waiting do Playwright (`page.getByRole(...)`) em vez de timeouts manuais (`page.waitForTimeout()`).
- **Critérios de Aceitação Baseados nos Requisitos**:
  - Validar se o login redireciona corretamente para o Dashboard.
  - Validar se o preenchimento do Wizard permite avançar e salva rascunho com sucesso.
  - Validar se o perfil Contributor não consegue acessar as abas restritas do painel de administração.
