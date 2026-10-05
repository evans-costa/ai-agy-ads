---
trigger: glob
globs:
  - "tests/**/*"
description: "Padrões de testes automatizados com xUnit v3, Shouldly, Testcontainers.PostgreSql e CommonTestUtilities."
---

# Testes Automatizados e Qualidade de Software - Lessons Hub

> **Descrição**: Diretrizes para escrita de testes automatizados com xUnit v3, Shouldly, Testcontainers e utilitários de teste.

---

## Estrutura da Suíte de Testes

- **Framework de Testes**: Utilização de **xUnit v3** (`xunit.v3`).
- **Asserções Fluentes**: Utilização do **Shouldly** para asserções descritivas e legíveis (ex: `response.StatusCode.ShouldBe(HttpStatusCode.OK)` ou `result.Errors.ShouldContain(...)`).
- **Testes de Integração com Banco Real**: Os testes de integração da API (`LessonsHub.API.IntegrationTests`) utilizam **Testcontainers** (`Testcontainers.PostgreSql`) para instanciar containers Docker temporários do PostgreSQL durante os testes. **Nunca utilize bancos de dados em memória (EF Core InMemory)**.

---

## Organização dos Projetos de Teste

A estrutura de testes divide-se em três projetos:

1. **`tests/CommonTestUtilities`**:
   - Biblioteca compartilhada com utilitários de apoio a testes.
   - Contém builders de entidades, geradores de dados para testes, mocks de repositórios e serviços de criptografia/token.
2. **`tests/LessonsHub.Application.Tests`**:
   - Testes unitários para a camada de aplicação.
   - Focado na validação de Casos de Uso (`UseCases`) e Validadores (`FluentValidation`).
3. **`tests/LessonsHub.API.IntegrationTests`**:
   - Testes de integração de ponta a ponta da API HTTP.
   - Utiliza `WebApplicationFactory` customizada e Testcontainers para testar o ciclo completo da requisição HTTP até o banco PostgreSQL.

---

## Boas Práticas e Testes Contínuos

- **Isolamento**: Testes unitários de casos de uso devem isolar dependências externas utilizando mocks e stubs.
- **Cobertura Contínua**: Todo novo caso de uso, regra de validação ou endpoint deve ser acompanhado de testes cobrindo o caminho feliz e os principais cenários de exceção (ex: credenciais inválidas, entidades não encontradas, dados ausentes).
- **Sem Falsos Positivos**: Escreva asserções detalhadas sobre as propriedades dos dados retornados e verifique se as exceções esperadas possuem os códigos de status e mensagens corretas.
