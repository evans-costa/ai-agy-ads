---
trigger: always_on
description: "Tratamento de erros da API (RFC 7807/9457 com atributo code), mapeamento de formulários no React Hook Form, Sonner toasts e Error Boundaries."
---

# Tratamento de Erros e Notificações - Frontend Lessons Hub

> **Descrição**: Tratamento de respostas RFC 7807/9457 (`ProblemDetails` e `ValidationProblemDetails`) com suporte ao atributo semântico `code`, mapeamento de erros em formulários, notificações com Sonner e Error Boundaries.

---

## 1. Contrato de Erros da API Backend (RFC 7807 / RFC 9457)

A API padroniza respostas de falha nos formatos `ProblemDetails` e `ValidationProblemDetails`:
- `code`: Identificador semântico unificado do tipo de erro (ex: `validation_error`, `conflict`, `unauthorized`, `unauthorized_access`, `forbidden_access`, `resource_not_found`).
- `status`: Código HTTP numérico da resposta.
- `title`: Resumo descritivo do problema.
- `detail`: Detalhamento específico (omitido em produção para erros 500 por segurança).
- `instance`: Rota/método da requisição originária.
- `errors`: Dicionário `Record<string, string[]>` com erros por campo (exclusivo do `ValidationProblemDetails` quando `code === "validation_error"`).
- `traceId`: Identificador da requisição para auditoria (`HttpContext.TraceIdentifier`).
- `timestamp`: Data/hora ISO 8601 UTC.

---

## 2. Mapeamento por Código de Erro (`code`) e Comportamento

> **Regra Mandatória**: O frontend deve discriminar erros prioritariamente pelo atributo `code` (`ApiErrorCode`), utilizando o código numérico `status` apenas como fallback para falhas de infraestrutura ou exceções genéricas sem código mapeado.

| `code` | Código HTTP | Exceção Backend / Origem | Resposta | Ação no Frontend |
|---|---|---|---|---|
| `validation_error` | **400 Bad Request** | `ValidationException` | `ValidationProblemDetails` | Mapear mensagens nos inputs via `applyFieldErrors` / `setError` e toast de alerta |
| `unauthorized` | **401 Unauthorized** | `UnauthorizedException` | `ProblemDetails` | Alerta no login de credenciais inválidas (não remove token existente) |
| `unauthorized_access` | **401 Unauthorized** | JWT `OnChallenge` (token ausente/expirado) | `ProblemDetails` | Interceptor limpa sessão (`access_token`, `user_profile`) e redireciona para `/login` |
| `forbidden_access` | **403 Forbidden** | JWT `OnForbidden` / `ForbiddenException` | `ProblemDetails` | Toast de acesso negado e bloqueio de navegação |
| `resource_not_found` | **404 Not Found** | `NotFoundException` | `ProblemDetails` | Mensagem contextual informando recurso não localizado |
| `conflict` | **409 Conflict** | `ConflictException` | `ProblemDetails` | Aviso contextual de duplicidade ou atribuição ao campo em conflito |
| *(sem code)* | **400 Bad Request** | `BadHttpRequestException` | `ProblemDetails` | Toast de alerta com a descrição do erro |
| *(sem code)* | **500 Internal Error** | `System.Exception` | `ProblemDetails` | Toast amigável: *"Não foi possível processar a requisição no momento."* |

---

## 3. Mapeamento em Formulários (`React Hook Form`)

Ao receber erro com `code === ApiErrorCode.ValidationError` e `errors: Record<string, string[]>`:
1. Converta as chaves de campo do backend (PascalCase, ex: `Email`) para camelCase do schema (ex: `email`) através do utilitário `parseApiError`.
2. Injete cada erro no formulário via `applyFieldErrors(apiError, setError, formFields)`.
3. Notifique com `toast.error(apiError.detail || "Por favor, corrija os erros no formulário.")`.
4. Priorize a indicação visual nos campos; evite abrir múltiplos toasts individuais para cada campo.

---

## 4. Notificações Visuais com Sonner

Alertas flutuantes devem respeitar a paleta do Design System:
- **Sucesso (`toast.success`)**: Borda/ícone verde (`#10B981` ou `#15803D`).
- **Erro (`toast.error`)**: Borda/destaque vermelho alerta (`#FF2F52`).
- **Aviso (`toast.warning`)**: Borda/destaque âmbar (`#F59E0B`).
- **Informativo (`toast.info`)**: Borda/destaque azul primário (`#0055FF`).

---

## 5. Error Boundaries no TanStack Router

Rotas principais devem declarar `errorComponent` com:
- Ícone de alerta e título objetivo.
- **Botão Primário**: "Tentar novamente" (`reset()` ou reload da rota).
- **Botão Secundário**: "Voltar ao Início" (redirecionamento para `/`).
