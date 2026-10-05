---
trigger: glob
globs:
  - "LessonsHub.Domain/**/*"
  - "LessonsHub.Infrastructure/DataAccess/Configurations/**/*"
description: "Diretrizes e padrões para criação e evolução de entidades POCO, enums e contratos de repositório no Domain."
---

# Entidades de Domínio e Enums - Lessons Hub

> **Descrição**: Diretrizes para criação e modificação de entidades e enums de negócio no Lessons Hub.

---

## Estrutura das Entidades de Domínio

As entidades de domínio residem em `LessonsHub.Domain/Entities` e são implementadas como classes POCO limpas:

- **Propriedades Fortemente Tipadas**: Utilize tipos semânticos adequados (`Guid`, `int`, `string`, `DateTime`, enums).
- **Inicialização de Coleções**: Inicialize propriedades de navegação do tipo coleção por padrão para evitar referências nulas:
  ```csharp
  public ICollection<LessonAction> Actions { get; set; } = new List<LessonAction>();
  public ICollection<LessonTag> LessonTags { get; set; } = new List<LessonTag>();
  ```
- **Auditoria Temporal**: Entidades de fluxo operacional devem manter propriedades temporais (`CreatedAt`, `UpdatedAt?`).

---

## Convenções de Identificadores (Chaves Primárias)

- **Identificadores UUID/GUID**: Utilizados nas entidades principais e de fluxo do sistema (`Lesson`, `Project`, `User`, `LessonAction`, `Tag`), garantindo unicidade distribuída.
- **Identificadores Inteiros (`int`)**: Utilizados exclusivamente para tabelas de catálogo/lookup (`Category`).

---

## Enums de Negócio

Os estados e papéis de negócio devem ser estritamente tipados via Enums em `LessonsHub.Domain/Enums`:

1. **`LessonStatus`**:
   - `Draft`: Rascunho de lição sendo elaborada.
   - `ActionPending`: Publicada com ações corretivas/preventivas em aberto.
   - `Institutionalized`: Ações concluídas com evidências registradas (Double-Loop).
   - `Archived`: Lição arquivada.

2. **`LessonActionStatus`**:
   - `Pending`: Ação planejada aguardando início.
   - `InProgress`: Ação em execução.
   - `Completed`: Ação finalizada com evidência descritiva obrigatória.
   - `Cancelled`: Ação cancelada ou substituída.

3. **`ProjectStatus`**:
   - `Planning`: Projeto em planejamento.
   - `InProgress`: Projeto em andamento.
   - `Completed`: Projeto concluído.
   - `Cancelled`: Projeto cancelado.

4. **`UserRole`**:
   - `Admin`: Gestão completa de usuários, permissões e parâmetros do sistema.
   - `Manager`: Gestão de projetos, categorias, curadoria de lições e métricas.
   - `Contributor`: Cadastro de lições, consulta de acervo e execução de ações.
