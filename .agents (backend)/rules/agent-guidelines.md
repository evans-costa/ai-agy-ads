---
trigger: always_on
description: "Diretrizes específicas de desenvolvimento backend (.NET 10 / C#), camadas da Clean Architecture e convenções de nomenclatura."
---

# Diretrizes e Fluxo de Trabalho do Agente - Backend Lessons Hub

> **Descrição**: Diretrizes e regras essenciais para agentes de IA que desenvolvem o backend do Lessons Hub, definindo ordem de prioridade, processos antes da implementação e convenções C#/.NET.

---

## 1. Ordem de Prioridade

Em caso de instruções conflitantes, siga rigorosamente esta ordem de prioridade:

1. **Instruções explícitas do usuário** enviadas no chat.
2. **`AGENTS.md` raiz** (`.agents/AGENTS.md`).
3. **`AGENTS.md` do backend** (`codigo-fonte/backend/.agents/AGENTS.md`).
4. **Arquivos de regras específicos** na pasta local `.agents/rules/`.
5. **Convenções implícitas do ecossistema** (.NET 10 / C#).

---

## 2. Antes de Implementar

Sempre execute as etapas abaixo antes de criar ou modificar qualquer arquivo de código:

1. **Identificar a camada correta**: determine se a alteração pertence a `LessonsHub.Domain`, `LessonsHub.Application`, `LessonsHub.Communication`, `LessonsHub.Infrastructure`, `LessonsHub.API` ou `LessonsHub.Exception`.
2. **Consultar as regras específicas**: leia os arquivos de regras correspondentes ao escopo da tarefa (ex: [application-usecases.md](./application-usecases.md) para casos de uso, [domain-entities.md](./domain-entities.md) para modelos, [database-repository.md](./database-repository.md) para persistência).
3. **Verificar padrões existentes no projeto**: examine a base de código para manter consistência nos padrões arquiteturais estabelecidos.
4. **Respeitar a Clean Architecture**: garanta que camadas internas nunca dependam de camadas externas.

---

## 3. Convenções de Nomenclatura e Código (.NET / C#)

- **Estrutura de Arquivos**: Não crie arquivos ou pastas fora do padrão existente na solução.
- **Convenções de Nomenclatura**:
  - **Casos de Uso**: `I<Ação>UseCase` e `<Ação>UseCase` (ex: `IGetUserProfileUseCase`, `GetUserProfileUseCase`).
  - **DTOs / Contratos**: `Request<Ação>Json` e `Response<Ação>Json` em `LessonsHub.Communication`.
  - **Validadores**: `<Request>Validator` herdando de `AbstractValidator<T>` via FluentValidation.
  - **Repositórios**: `I<Entidade>ReadOnlyRepository` e `I<Entidade>WriteOnlyRepository`.
- **Reutilização**: Sempre prefira reaproveitar classes, utilitários, enums ou DTOs já existentes.
- **Gestão de Pacotes NuGet**: A instalação e atualização de pacotes NuGet é responsabilidade manual do usuário (consulte [user-preferences.md](../../../../.agents/rules/user-preferences.md)).
- **Validação de Comportamento**: Nunca assuma regras de negócio ou de infraestrutura; havendo dúvidas, valide com o usuário.
- **Formatação de Código**: Antes de finalizar uma tarefa, garanta que o código esteja formatado via `dotnet format`.
