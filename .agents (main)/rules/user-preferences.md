---
trigger: always_on
description: "Aprovação prévia obrigatória de alterações pelo usuário, integridade de linters e gestão estritamente manual de pacotes npm e NuGet."
---

# Preferências do Usuário e Políticas de Autorização (Global)

> **Descrição**: Regras globais de governança sobre autorização prévia de mudanças pelo usuário, integridade de configurações de linter e gestão estritamente manual de dependências (npm e NuGet).

---

## 1. Aprovação Obrigatória de Mudanças

Antes de modificar arquivos essenciais, criar novas rotas/features, executar migrações de banco de dados ou rodar comandos que alterem o estado do repositório:

1. **Apresente um resumo claro e conciso**:
   - O que será realizado;
   - Quais arquivos serão criados, modificados ou excluídos;
   - Os impactos previstos na solução.
2. **Aguarde a confirmação e aprovação explícita do usuário** antes de executar a alteração.

---

## 2. Gestão Estritamente Manual de Dependências e Pacotes

A instalação, atualização ou remoção de dependências é de **responsabilidade exclusiva do usuário**, tanto no backend quanto no frontend:

- **Proibição Estrita de Automação**:
  - O agente **NUNCA** deve executar comandos como `npm install`, `npm add`, `npm update`, `dotnet add package`, nem editar manualmente as seções de dependências de `package.json` ou arquivos `.csproj`.
- **Procedimento Obrigatório para Adicionar Dependências**:
  1. Justifique tecnicamente a real necessidade da biblioteca.
  2. Forneça o comando exato para o usuário executar no terminal:
     - **Backend (.NET / NuGet)**:
       ```bash
       dotnet add package <PackageName> --version <Version> --project <ProjectPath>
       ```
     - **Frontend (Node / npm)**:
       ```bash
       npm install <pacote>
       # ou para desenvolvimento:
       npm install -D <pacote>
       ```
  3. Aguarde a confirmação explícita de que o usuário realizou a instalação antes de prosseguir com a escrita do código dependente.

---

## 3. Integridade de Ferramental, Linters e Biome

- **Arquivos de Configuração de Linter**: **NUNCA** altere arquivos como `biome.jsonc`, `.editorconfig` ou similares de forma autônoma. Qualquer alteração exige consentimento prévio do usuário.
- **Zero Supressões Arbitrárias**: **NUNCA** insira comentários de supressão de linter (ex: `// biome-ignore ...` ou `#pragma warning disable`) para mascarar erros. Todos os diagnósticos de qualidade devem ser resolvidos na arquitetura e implementação limpa do código.
