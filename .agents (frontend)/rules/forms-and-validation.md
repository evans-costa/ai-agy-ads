---
trigger: always_on
description: "React Hook Form com validação Zod, feedback visual de erros e fluxo do Wizard em 3 etapas."
---

# Formulários e Validação - Frontend Lessons Hub

> **Descrição**: Construção de formulários com React Hook Form, validação de esquemas fortemente tipados com Zod e formulário em etapas (Wizard).

---

## 1. Stack e Padrão de Formulários

Todos os formulários devem integrar:
- **React Hook Form**: Gerenciamento de estado sem re-renderizações desnecessárias.
- **Zod**: Schemas declarativos com inferência automática de tipos (`z.infer<typeof schema>`).
- **Resolver**: `@hookform/resolvers/zod`.
- **Localização de Schemas**: Pasta `types/` de cada feature, espelhando com precisão os contratos DTO e regras FluentValidation do backend.

---

## 2. Feedback Visual de Erros nos Campos

Os componentes de entrada (`Input`, `Select`, `Textarea`) devem refletir:
1. **Borda e Ring de Erro**: `border-danger focus-visible:ring-danger text-danger` quando houver erro no campo.
2. **Mensagem Descritiva**: Exibir mensagem abaixo do campo em `text-xs text-danger`.
3. **Prevenção de Duplo Envio**: Desabilitar o botão primário (`disabled={isSubmitting}`) e exibir estado de loading durante a submissão.

---

## 3. Gestão do Wizard em 3 Etapas (`src/features/lessons/components/Wizard/`)

1. **Etapas**:
   - **Etapa 1 (AAR)**: Título, Projeto, Categoria, O que foi planejado, Ocorrido, Lições aprendidas.
   - **Etapa 2 (RCA)**: Diagnóstico dos 5 Porquês e Severidade do impacto.
   - **Etapa 3 (PDCA)**: Definição dos planos de ação com responsável e prazo.
2. **Preservação de Estado**: Utilize `FormProvider` ou estado centralizado no componente pai para preservar valores preenchidos ao navegar entre passos (`Continuar →` / `← Voltar`).
3. **Validação Parcial por Etapa**: Antes de avançar para o próximo passo, execute validação estrita apenas dos campos da etapa atual via `trigger(['campo1', 'campo2'])`.
4. **Salvar Rascunho**: O botão `Salvar Rascunho` deve persistir a lição com status `Draft` mesmo que as etapas posteriores não estejam completas.
