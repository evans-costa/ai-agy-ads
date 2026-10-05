---
trigger: always_on
description: "Estratégia de branches GitHub Flow, padronização de Conventional Commits, autoria de commits e checklist de qualidade pré-commit."
---

# Diretrizes de Fluxo Git e Controle de Versão (Global)

> **Descrição**: Estratégia de branches baseada no GitHub Flow, convenções semânticas de commits (Conventional Commits), autoria de alterações e checklist pré-commit para todas as camadas do Lessons Hub.

---

## 1. Fluxo Central: GitHub Flow

O projeto adota o padrão **GitHub Flow** como estratégia oficial de ramificação e integração contínua:

1. **A ramificação `main` é sagrada e estável**:
   - O código em `main` deve estar sempre compilável, tipado e com a suíte de testes passando.
   - **É estritamente proibido fazer commits diretos na `main`**. Toda alteração deve ser desenvolvida em uma branch separada e integrada via Pull Request (PR).
2. **Crie uma branch a partir da `main` atualizada**:
   ```bash
   git checkout main
   git pull
   git checkout -b <prefixo>/<descricao-curta>
   ```
3. **Faça commits atômicos com Conventional Commits**:
   - Salve alterações pequenas, incrementais e autocontidas com mensagens claras e semânticas.
4. **Abra um Pull Request (PR)**:
   - Abra o PR para expor a proposta e solicitar revisão de código (Code Review) pelos colegas de equipe.
5. **Revisão e Testes**:
   - Resolva apontamentos de revisão e assegure que o pipeline/testes automatizados passem com 100% de sucesso.
6. **Merge na `main`**:
   - Uma vez aprovado e verificado, faça o merge da branch na `main`.

---

## 2. Prefixos e Nomenclatura de Branches

Utilize nomes em minúsculas com palavras separadas por hífen (`kebab-case`):

| Prefixo | Quando utilizar | Exemplo |
| :--- | :--- | :--- |
| `feature/` | Nova funcionalidade, tela, componente ou caso de uso | `feature/register-lesson-usecase` ou `feature/wizard-step-2` |
| `fix/` | Correção de bug, falha identificada ou erro de layout | `fix/token-expiration-validation` ou `fix/modal-overflow` |
| `refactor/` | Refatoração de código sem alteração de comportamento externo | `refactor/user-repository-queries` ou `refactor/button-variants` |
| `test/` | Adição ou aprimoramento de testes automatizados | `test/lesson-action-integration` ou `test/wizard-navigation` |
| `style/` | Ajustes visuais, CSS, Tailwind ou espaçamentos sem impacto lógico | `style/dashboard-metric-card-padding` |
| `docs/` | Atualizações em documentação, regras de agentes ou especificações | `docs/update-api-readme` ou `docs/design-system-tokens` |
| `chore/` | Tarefas de manutenção (dependências, scripts, linter, build) | `chore/update-nuget-packages` ou `chore/vite-path-alias` |

---

## 3. Padrão de Mensagens (Conventional Commits)

Formato obrigatório: `tipo(escopo opcional): descrição clara no imperativo`

| Tipo | Finalidade | Exemplo |
| :--- | :--- | :--- |
| `feat` | Nova funcionalidade para o usuário ou API | `feat(lessons): implement register lesson use case` |
| `fix` | Correção de bug | `fix(auth): redirect to login on 401 token expiration` |
| `refactor` | Refatoração interna de código sem alteração de regra | `refactor(ui): extract reusable modal component` |
| `test` | Inclusão ou ajuste de testes automatizados | `test(users): add integration tests for user registration endpoint` |
| `style` | Ajustes puramente visuais, formatação ou estilos Tailwind | `style(dashboard): adjust metric card padding` |
| `docs` | Modificações exclusivamente em documentação | `docs(readme): add instructions for running docker compose` |
| `chore` | Alterações em tarefas de build, dependências ou ferramentas | `chore(deps): update vite configuration` |

### Escopos Comuns Recomendados:
- **Backend**: `(auth)`, `(lessons)`, `(users)`, `(categories)`, `(projects)`, `(database)`, `(exceptions)`.
- **Frontend**: `(auth)`, `(dashboard)`, `(wizard)`, `(admin)`, `(ui)`, `(router)`, `(queries)`.

---

## 4. Checklist Pré-Commit por Stack

Antes de preparar qualquer commit, certifique-se de que a respectiva stack passa nos critérios de qualidade:

### No Backend (`codigo-fonte/backend`):
1. **Compilação**: `dotnet build LessonsHub.slnx` sem erros.
2. **Formatação**: `dotnet format LessonsHub.slnx` aplicado.
3. **Testes**: `dotnet test LessonsHub.slnx` com 100% de aprovação.

### No Frontend (`codigo-fonte/frontend`):
1. **Tipos estritos**: `npx tsc --noEmit` executa com zero erros.
2. **Linter e Padrões**: `npm run lint` ou `npx ultracite check` sem avisos ou erros.
3. **Testes**: `npm run test` com todos os testes unitários/componentes passando.

---

## 5. Autoria de Commits

O Lessons Hub é um projeto colaborativo desenvolvido em equipe:
- A autoria de cada commit fica sob responsabilidade do integrante que estiver realizando a alteração, utilizando a sua respectiva configuração local do Git (`git config user.name` e `git config user.email`).

---

## 6. Atualização Documental Contínua

Sempre que uma alteração incluir, remover ou modificar regras de negócio, contratos de API, tokens de interface ou parâmetros de configuração:
- Atualize a respectiva documentação (ex: `README.md`, `.agents/rules/` ou documentos em `documentos/`) **no mesmo ciclo de desenvolvimento do PR**.
