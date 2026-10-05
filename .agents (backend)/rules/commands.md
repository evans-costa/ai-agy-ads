---
trigger: always_on
---

# Comandos de Execução e CLI - Lessons Hub

> **Descrição**: Referência rápida dos comandos CLI para compilação, execução local, testes automatizados, migrações de banco de dados e formatação na solução Lessons Hub.

---

## 1. Compilação e Build

Compile toda a solução a partir da raiz da pasta `codigo-fonte/backend`:

```bash
# Compilar a solução completa
dotnet build LessonsHub.slnx
```

---

## 2. Execução Local da API

Inicie o servidor local da API ASP.NET Core:

```bash
dotnet run --project LessonsHub.API
```

- Swagger / OpenAPI estará acessível em: `https://localhost:7115/docs/v1` (ou na porta configurada em `launchSettings.json`).

---

## 3. Testes Automatizados

### Executar Toda a Suíte de Testes
```bash
dotnet test --solution LessonsHub.slnx
```

### Executar Projetos Específicos de Teste
```bash
# Testes unitários da camada de aplicação (Use Cases e Validadores)
dotnet test --project tests/LessonsHub.Application.Tests

# Testes de integração de API (Testcontainers e PostgreSQL real)
dotnet test --project tests/LessonsHub.API.IntegrationTests
```

### Filtrar Testes Específicos
```bash
dotnet test --project tests/LessonsHub.Application.Tests --filter "FullyQualifiedName~NomeDoTesteOuClasse"
```

---

## 4. Banco de Dados e Migrações (EF Core)

A persistência do Lessons Hub utiliza PostgreSQL via Entity Framework Core.

### Subir Banco de Dados de Desenvolvimento (Docker)
Inicia o container PostgreSQL configurado:

```bash
docker compose -f LessonsHub.API/docker-compose.yml up -d
```

### Criar Nova Migração
Gera uma nova migração com as alterações das entidades:

```bash
dotnet ef migrations add <NomeDaMigracao> --project LessonsHub.Infrastructure --startup-project LessonsHub.API
```

### Aplicar Migrações no Banco de Dados
Aplica migrações pendentes ao banco local:

```bash
dotnet ef database update --project LessonsHub.Infrastructure --startup-project LessonsHub.API
```

---

## 5. Formatação de Código e Linter

Verifique e aplique a padronização de formatação antes de preparar commits:

```bash
dotnet format LessonsHub.slnx
```
