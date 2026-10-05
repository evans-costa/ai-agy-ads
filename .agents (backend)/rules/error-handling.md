---
trigger: glob
globs:
  - "src/LessonsHub.Exception/**/*"
  - "src/LessonsHub.API/Handlers/**/*"
description: "Tratamento global de exceções com IExceptionHandler (GlobalExceptionHandler), LessonsHubException, ProblemDetails (RFC 7807 / 9110), ErrorCodes e mensagens centralizadas em .resx."
---

# Tratamento de Exceções e Mensagens de Erro - Lessons Hub

> **Descrição**: Tratamento centralizado de exceções com `IExceptionHandler` (`GlobalExceptionHandler`), hierarquia `LessonsHubException` com primary constructor, catálogo de códigos legíveis por máquinas em `ErrorCodes`, respostas RFC 7807 / RFC 9110 via `ProblemDetails` e mensagens centralizadas em `.resx` (neutro e `pt-BR`).

---

## 1. Pipeline de Exceções (`GlobalExceptionHandler`)

A API adota um pipeline centralizado de tratamento de falhas implementando `Microsoft.AspNetCore.Diagnostics.IExceptionHandler`, configurado no `Program.cs` via:

```csharp
builder.Services.AddProblemDetails(options =>
{
    options.CustomizeProblemDetails = ctx =>
    {
        ctx.ProblemDetails.Extensions["traceId"] = Activity.Current?.Id ?? ctx.HttpContext.TraceIdentifier;
        ctx.ProblemDetails.Extensions["timestamp"] = DateTime.UtcNow;
        ctx.ProblemDetails.Instance = $"{ctx.HttpContext.Request.Method} {ctx.HttpContext.Request.Path}";
    };
});

builder.Services.AddExceptionHandler<GlobalExceptionHandler>();
```

No pipeline HTTP (`WebApplication`):
- `app.UseExceptionHandler()`: Ativa o manipulador global.
- `app.UseStatusCodePages()`: Garante que respostas com status code puro fora de controllers (ex: 401/403 emitidos por autenticação/autorização JWT ou 404 de rotas inexistentes) sejam padronizadas no formato `ProblemDetails`.

### Fluxo de Execução do `GlobalExceptionHandler`

O [`GlobalExceptionHandler`](src/LessonsHub.API/Handlers/GlobalExceptionHandler.cs) implementa `TryHandleAsync` e processa as exceções via pattern matching:

1. **`ValidationException`**:
   - Cria uma instância de `HttpValidationProblemDetails(exception.Errors)`.
   - Define status `400 Bad Request`, título `exception.Title` (`"Validation failed"`), detalhe `exception.Message` (`ResourceErrorMessages.VALIDATION_ERROR`), `Instance = httpContext.Request.Path` e inclui a extensão `Extensions["code"] = exception.ErrorCode` (padrão `"validation_error"`).
2. **`LessonsHubException`**:
   - Trata exceções previsíveis de negócio e aplicação (`NotFoundException`, `ConflictException`, `UnauthorizedException`).
   - Define status `(int)exception.StatusCode`, título `exception.Title`, detalhe `exception.Message`, `Instance = httpContext.Request.Path` e extensão `Extensions["code"] = exception.ErrorCode`.
3. **`BadHttpRequestException`**:
   - Trata requisições malformadas ou com corpo inválido.
   - Define status `exception.StatusCode` (geralmente 400), título `"Bad request"`, `Instance = httpContext.Request.Path`, e detalhe condicional: mensagem original da exceção em ambiente de desenvolvimento ou `ResourceErrorMessages.BAD_REQUEST_ERROR` em produção.
4. **Exceções Não Mapeadas (`System.Exception`)**:
   - Captura qualquer falha inesperada.
   - Retorna status `500 Internal Server Error`, título `"Internal server error"`, `Instance = httpContext.Request.Path`, e detalhe condicional: mensagem original em desenvolvimento ou `ResourceErrorMessages.UNKNOWN_ERROR` em produção.

### Escrita da Resposta e Fallback

O handler prioriza a escrita via `IProblemDetailsService.TryWriteAsync(...)`. Caso o serviço não consiga escrever o payload, é acionado um fallback explícito via:

```csharp
await httpContext.Response.WriteAsJsonAsync(
    problemDetails,
    options: null,
    contentType: "application/problem+json",
    cancellationToken: cancellationToken);
```

### Registro de Logs Estruturados

- **Falhas de Servidor (`statusCode >= 500`)**: Logadas como `LogError` com a exceção completa, método, caminho e `TraceIdentifier`:
  `"Unhandled exception occurred on {Method} {Path}. TraceId: {TraceId}"`
- **Erros de Cliente / Negócio (`statusCode < 500`)**: Logados como `LogInformation`:
  `"{ExceptionType} handled with status code {StatusCode} on {Method} {Path}. TraceId: {TraceId}"`

---

## 2. Catálogo de Códigos de Erro (`ErrorCodes`)

Para desacoplar o frontend de strings localizadas e permitir tratamento programático e assertivo de erros, a solução define a classe estática [`ErrorCodes`](src/LessonsHub.Exception/ErrorCodes.cs):

```csharp
namespace LessonsHub.Exception;

public static class ErrorCodes
{
    public const string ValidationError = "validation_error";
    public const string Conflict = "conflict";
    public const string ResourceNotFound = "resource_not_found";
    public const string Unauthorized = "unauthorized";
    public const string UnauthorizedAccess = "unauthorized_access";
    public const string ForbiddenAccess = "forbidden_access";

    public static class Users
    {
        public const string EmailAlreadyExists = "users.email_already_exists";
    }
}
```

O código é exposto na chave `"code"` do payload `ProblemDetails` e deve ser preferencialmente referenciado a partir dessas constantes.

---

## 3. Hierarquia de Exceções (`LessonsHubException`)

Todas as exceções previsíveis de negócio e validação herdam de [`LessonsHubException`](src/LessonsHub.Exception/ExceptionBase/LessonsHubException.cs), utilizando *primary constructor* do C#:

```csharp
using System.Net;

namespace LessonsHub.Exception.ExceptionBase;

public abstract class LessonsHubException(
    string message,
    HttpStatusCode statusCode,
    string title,
    string errorCode)
    : System.Exception(message)
{
    public HttpStatusCode StatusCode { get; } = statusCode;

    public string Title { get; } = title;

    public string ErrorCode { get; } = errorCode;
}
```

### Tipos Concretos de Exceção

| Exceção | Status HTTP | Title Padrão | ErrorCode Padrão | Assinaturas e Uso |
| :--- | :--- | :--- | :--- | :--- |
| `ValidationException` | `400 Bad Request` | `"Validation failed"` | `ErrorCodes.ValidationError` | `new ValidationException(errors, errorCode?)`<br/>`new ValidationException(field, error, errorCode?)` |
| `UnauthorizedException` | `401 Unauthorized` | `"Unauthorized"` | `ErrorCodes.Unauthorized` | `new UnauthorizedException(message, errorCode?)`<br/>Usada em falhas de login (`ResourceErrorMessages.INVALID_LOGIN`) ou token ausente/inválido. |
| `NotFoundException` | `404 Not Found` | `"Resource not found"` | `ErrorCodes.ResourceNotFound` | `new NotFoundException(resourceName, key, errorCode?)`<br/>`new NotFoundException(message, errorCode?)`<br/>`new NotFoundException()` |
| `ConflictException` | `409 Conflict` | `"Conflict"` | `ErrorCodes.Conflict` | `new ConflictException(message, errorCode?)`<br/>Ex: `new ConflictException(ResourceErrorMessages.EMAIL_ALREADY_EXISTS, ErrorCodes.Users.EmailAlreadyExists)` |

---

## 4. Contrato de Resposta Padronizada (`ProblemDetails`)

A API retorna exclusivamente respostas de erro aderentes a **RFC 7807** e **RFC 9110**.

### Exemplo 1: Recurso Não Encontrado (404 Not Found)

```json
{
  "status": 404,
  "title": "Resource not found",
  "detail": "User with identifier 01923456-7890-7abc-def0-1234567890ab was not found",
  "instance": "GET /api/v1/users/01923456-7890-7abc-def0-1234567890ab",
  "code": "resource_not_found",
  "traceId": "0HN1234567890",
  "timestamp": "2026-10-05T19:00:00Z"
}
```

### Exemplo 2: Conflito de Unicidade de Negócio (409 Conflict)

```json
{
  "status": 409,
  "title": "Conflict",
  "detail": "Email already exists",
  "instance": "POST /api/v1/users",
  "code": "users.email_already_exists",
  "traceId": "0HN1234567890",
  "timestamp": "2026-10-05T19:00:00Z"
}
```

### Exemplo 3: Falha de Validação (400 Bad Request com `HttpValidationProblemDetails`)

```json
{
  "status": 400,
  "title": "Validation failed",
  "detail": "One or more validation errors occurred.",
  "instance": "POST /api/v1/auth/login",
  "code": "validation_error",
  "errors": {
    "Email": [
      "Invalid email format"
    ]
  },
  "traceId": "0HN1234567890",
  "timestamp": "2026-10-05T19:00:00Z"
}
```

---

## 5. Centralização e Internacionalização de Mensagens (.resx)

Todas as mensagens legíveis voltadas ao cliente devem residir na camada `LessonsHub.Exception`:

- `src/LessonsHub.Exception/ResourceErrorMessages.resx`: Arquivo neutro / padrão (inglês).
- `src/LessonsHub.Exception/ResourceErrorMessages.pt-br.resx`: Localização em português (pt-BR).

### Regras Obrigatórias de Desenvolvimento

1. **Zero Mensagens Hardcoded**: Nunca introduza mensagens de erro literais em Use Cases, Validadores FluentValidation ou Controllers. Sempre crie a chave no `.resx` correspondente.
2. **Nomenclatura UPPER_SNAKE_CASE**: Todas as chaves em arquivos de recursos devem usar letras maiúsculas e sublinhado (ex: `USER_NOT_FOUND`, `EMAIL_ALREADY_EXISTS`, `INVALID_LOGIN`, `VALIDATION_ERROR`, `ENTITY_NOT_FOUND`).
3. **Parâmetros Dinâmicos**: Em mensagens compostas, adicione `{0}`, `{1}` ao recurso e utilize a sobrecarga da exceção ou formate com `string.Format`:
   ```csharp
   // Sobrecarga de conveniência em NotFoundException:
   throw new NotFoundException(nameof(User), id);

   // Ou manualmente:
   string.Format(ResourceErrorMessages.ENTITY_NOT_FOUND, nameof(User), id);
   ```
