---
trigger: glob
globs:
  - "LessonsHub.Infrastructure/**/*"
  - "LessonsHub.Domain/Repositories/**/*"
description: "Diretrizes de persistência com EF Core, PostgreSQL, mapeamento fluente e implementação de repositórios."
---

# Banco de Dados, Repositórios e Unidade de Trabalho - Lessons Hub

> **Descrição**: Acesso a dados com EF Core e PostgreSQL, segregação de interfaces de repositório, persistência com IUnitOfWork e configurações de conexão.

---

## Repositórios e Segregação por Intenção

O acesso a dados e persistência no PostgreSQL deve ser realizado exclusivamente através de interfaces de repositórios no domínio, segregadas por intenção de leitura e escrita:

1. **Repositórios de Leitura (`ReadOnly`)**:
   - Nomenclatura: `I<Entidade>ReadOnlyRepository` (ex: `IUserReadOnlyRepository`, `ILessonReadOnlyRepository`).
   - Métodos otimizados para consulta (usando `AsNoTracking()`).
   - Exemplo: `GetById`, `GetByEmail`, `ExistsByEmail`, `ListAll`.

2. **Repositórios de Escrita (`WriteOnly`)**:
   - Nomenclatura: `I<Entidade>WriteOnlyRepository` (ex: `IUserWriteOnlyRepository`, `ILessonWriteOnlyRepository`).
   - Métodos responsáveis por anexar ou alterar estados de entidades no contexto (`Add`, `Update`, `Delete`).
   - Não realizam `SaveChanges` internamente.

---

## Persistência e Transações (IUnitOfWork)

Após criar, modificar ou excluir qualquer entidade via repositório de escrita:

- **Não** chame métodos de persistência do contexto EF Core diretamente nos casos de uso.
- A confirmação da transação deve ser delegada à interface `IUnitOfWork` injetada:
    ```csharp
    await _unitOfWork.SaveChangesAsync();
    ```

---

## DbContext e Mapeamento Fluente

- O contexto do banco é o `LessonsHubDbContext` localizado em `LessonsHub.Infrastructure/DataAccess`.
- As configurações de tabelas, índices (incluindo índices GIN e busca textual `tsvector`), tipos de colunas e relacionamentos devem residir na pasta `Configurations/` implementando `IEntityTypeConfiguration<T>`.

---

## Configurações de Conexão e Segurança

As configurações de ambiente devem seguir a estrutura no arquivo `appsettings.json` ou variáveis de ambiente:

- **Conexão PostgreSQL**:
    ```json
    "ConnectionStrings": {
      "DefaultConnection": "Host=localhost;Port=5432;Database=lessonshub;Username=postgres;Password=postgres"
    }
    ```
- **Configurações de JWT**:
    ```json
    "JwtSettings": {
      "SigningKey": "<chave-secreta-com-mais-de-32-caracteres>",
      "Issuer": "LessonsHub.API",
      "Audience": "LessonsHub.Client",
      "ExpiresInMinutes": 120
    }
    ```
