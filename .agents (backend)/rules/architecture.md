---
trigger: always_on
---

# Clean Architecture e Dependências de Camadas - Lessons Hub

> **Descrição**: Arquitetura Clean Architecture do Lessons Hub, responsabilidades de cada camada (Domain, Application, Communication, Infrastructure, API, Exception) e injeção de dependência.

---

## Regras de Dependência entre Camadas

A solução adota rigorosamente os princípios da Clean Architecture, onde camadas internas desconhecem detalhes das camadas externas:

```
LessonsHub.Exception      ──> (sem dependências internas de projetos)
LessonsHub.Communication  ──> (sem dependências internas de projetos)
LessonsHub.Domain         ──> LessonsHub.Exception
LessonsHub.Application    ──> LessonsHub.Domain + LessonsHub.Communication + LessonsHub.Exception
LessonsHub.Infrastructure ──> LessonsHub.Application + LessonsHub.Domain
LessonsHub.API            ──> LessonsHub.Application + LessonsHub.Infrastructure + LessonsHub.Communication + LessonsHub.Exception
```

---

## Responsabilidades de Cada Camada

### 1. LessonsHub.Exception
- Camada base isolada sem dependências com outros projetos da solução.
- Contém a classe base abstrata `LessonsHubException` e suas especializações (`NotFoundException`, exceções de validação, etc.).
- Centraliza mensagens de erro localizadas no arquivo `ResourceErrorMessages.resx` (pt-BR).

### 2. LessonsHub.Domain
- Núcleo de domínio do sistema.
- Contém entidades POCO (`Lesson`, `Project`, `Category`, `Tag`, `LessonAction`, `User`), enums de negócio (`LessonStatus`, `LessonActionStatus`, `ProjectStatus`, `UserRole`) e contratos de repositórios segregados por intenção.
- Contém interfaces de serviços de domínio e segurança (`IPasswordHasher`, `IAccessTokenGenerator`, `ITokenProvider`, `ILoggedUser`).

### 3. LessonsHub.Communication
- Camada de transporte de dados (DTOs) desacoplada.
- Define contratos de entrada (`Request...Json`) e saída (`Response...Json`, `ResponseErrorJson`) compartilháveis com o front-end.
- Não referencia nenhuma outra camada da solução.

### 4. LessonsHub.Application
- Contém a lógica de negócio dos casos de uso, organizada em `UseCases/<Entidade>/<Ação>/`.
- Cada caso de uso possui uma interface pública (`I<Ação>UseCase`) e uma classe executora (`<Ação>UseCase`).
- Contém validadores com `FluentValidation` para os DTOs de entrada.
- Orquestra repositórios do domínio e dispara persistência atômica via `IUnitOfWork`.

### 5. LessonsHub.Infrastructure
- Implementa o acesso a dados e serviços externos.
- Configurações do Entity Framework Core com provedor `Npgsql` para PostgreSQL (`LessonsHubDbContext`, mapeamentos fluentes e migrações).
- Implementação dos repositórios de leitura e escrita e do `IUnitOfWork`.
- Implementação de segurança: gerador de tokens JWT (`JwtTokenGenerator`), hashing de senhas (`PasswordHasher`) e contexto de usuário autenticado (`LoggedUser`).

### 6. LessonsHub.API
- Ponto de entrada da aplicação ASP.NET Core (.NET 10).
- Controllers HTTP herdando de `ControllerBase`.
- Middleware global de tratamento de exceções `GlobalExceptionHandler` (`IExceptionHandler`).
- Configuração de versionamento de API (`Asp.Versioning`), autenticação JWT Bearer, CORS e documentação OpenAPI / Swagger UI.

---

## Injeção de Dependências (DI)

As dependências de cada camada são registradas em métodos de extensão estáticos específicos:

- **Application**: registrado em `services.AddApplication()` na classe `LessonsHub.Application.DependencyInjectionExtension` (registra Casos de Uso e Validadores).
- **Infrastructure**: registrado em `services.AddInfrastructure(configuration, environment)` na classe `LessonsHub.Infrastructure.DependencyInjectionExtension` (registra DbContext, Repositórios, JWT, Criptografia e Serviços).
- Ambas as extensões são invocadas no `Program.cs` de `LessonsHub.API`.
