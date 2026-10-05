---
trigger: glob
globs:
  - "LessonsHub.Application/**/*"
  - "LessonsHub.Communication/**/*"
  - "LessonsHub.API/Controllers/**/*"
  - "tests/LessonsHub.Application.Tests/**/*"
description: "Padrões para Casos de Uso, validações com FluentValidation e DTOs na camada Application."
---

# Padrão de Casos de Uso (Use Cases) na Camada de Aplicação

> **Descrição**: Estruturação dos casos de uso na camada Application, injeção em Controllers, validação com FluentValidation e fluxo de execução.

---

## Organização dos Casos de Uso (Use Cases)

Toda a lógica de negócios da aplicação reside no projeto `LessonsHub.Application`, organizada dentro da pasta `UseCases/` agrupada por entidade e ação:

```
LessonsHub.Application/
└── UseCases/
    ├── Users/
    │   ├── GetProfile/
    │   │   ├── IGetUserProfileUseCase.cs
    │   │   └── GetUserProfileUseCase.cs
    │   └── Register/
    │       ├── IRegisterUserUseCase.cs
    │       ├── RegisterUserUseCase.cs
    │       └── RegisterUserValidator.cs
    └── Lessons/
        └── Register/
            ├── IRegisterLessonUseCase.cs
            ├── RegisterLessonUseCase.cs
            └── RegisterLessonValidator.cs
```

---

## Estrutura de um Caso de Uso

### 1. Interface
Cada caso de uso expõe uma interface pública com um método executor claro:

```csharp
namespace LessonsHub.Application.UseCases.Users.GetProfile;

public interface IGetUserProfileUseCase
{
    Task<ResponseUserProfileJson> Execute();
}
```

### 2. Implementação
A classe concreta implementa a interface, recebendo as dependências necessárias via injeção por construtor:

```csharp
namespace LessonsHub.Application.UseCases.Users.GetProfile;

public class GetUserProfileUseCase : IGetUserProfileUseCase
{
    private readonly ILoggedUser _loggedUser;

    public GetUserProfileUseCase(ILoggedUser loggedUser)
    {
        _loggedUser = loggedUser;
    }

    public async Task<ResponseUserProfileJson> Execute()
    {
        var user = await _loggedUser.Get();
        return new ResponseUserProfileJson
        {
            Id = user.Id,
            FullName = user.FullName,
            Email = user.Email,
            Role = user.Role
        };
    }
}
```

---

## Injeção e Execução nos Controllers

Os casos de uso são injetados diretamente nos Controllers correspondentes:

```csharp
[ApiController]
[Route("api/v{version:apiVersion}/[controller]")]
public class UsersController : ControllerBase
{
    [HttpGet("me")]
    [ProducesResponseType(typeof(ResponseUserProfileJson), StatusCodes.Status200OK)]
    public async Task<IActionResult> GetProfile([FromServices] IGetUserProfileUseCase useCase)
    {
        var response = await useCase.Execute();
        return Ok(response);
    }
}
```

---

## Validação de Entrada com FluentValidation

Para casos de uso que recebem dados de requisição (`Request...Json`), a validação de formato e regras básicas deve ser feita com `FluentValidation`:

- Crie uma classe herdando de `AbstractValidator<TRequest>` no mesmo diretório do caso de uso.
- Execute a validação no início do método `Execute`:
    ```csharp
    var validator = new RegisterLessonValidator();
    var result = await validator.ValidateAsync(request);

    if (!result.IsValid)
    {
        var errors = result.Errors
            .GroupBy(e => e.PropertyName)
            .ToDictionary(g => g.Key, g => g.Select(e => e.ErrorMessage).ToArray());

        throw new ValidationException(errors);
    }
    ```
- O erro lançado herda de `LessonsHubException`, sendo automaticamente capturado e formatado pelo `GlobalExceptionHandler` (`IExceptionHandler`).

---

## Comunicação e Retorno

- **DTOs de Entrada e Saída**: Devem sempre residir em `LessonsHub.Communication` (`Request...Json` e `Response...Json`).
- **Erros e Exceções**: Se uma regra de negócio for violada ou um registro não for encontrado, lance a exceção correspondente (`NotFoundException`, exceções derivadas de `LessonsHubException`). Nunca retorne `ActionResult` ou códigos HTTP diretamente a partir do Caso de Uso.
