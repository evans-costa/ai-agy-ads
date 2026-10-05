---
trigger: always_on
description: "Diretrizes universais de atuação do agente de IA, hierarquia de regras, checklist pré-implementação e integridade do código."
---

# Diretrizes Gerais e Conduta do Agente (Global)

> **Descrição**: Diretrizes fundamentais para agentes de IA atuando em qualquer parte do projeto Lessons Hub: hierarquia de prioridades, checklist antes de implementar e boas práticas operacionais.

---

## 1. Ordem de Prioridade

Em caso de instruções conflitantes, siga estritamente esta ordem de precedência:

1. **Instruções explícitas do usuário** enviadas na sessão atual.
2. **`AGENTS.md` raiz** (`.agents/AGENTS.md`).
3. **`AGENTS.md` específico da stack** (`codigo-fonte/backend/.agents/AGENTS.md` ou `codigo-fonte/frontend/.agents/AGENTS.md`).
4. **Arquivos de regras específicas** (ex: `rules/*.md`, `DESIGN.md`).
5. **Convenções implícitas dos frameworks e linguagens** (.NET 10 / C#, React 19 / TypeScript).

---

## 2. Checklist Pré-Implementação Universal

Antes de criar ou alterar qualquer arquivo de código ou documentação:

1. **Identificar a Stack e Camada Correta**:
   - Backend: certifique-se da camada adequada na Clean Architecture (`Domain`, `Application`, `Communication`, `Infrastructure`, `API`, `Exception`).
   - Frontend: determine o escopo modular feature-based (`src/features/<modulo>`, `src/components/ui`, `src/routes`, `src/shared`).
2. **Consultar as Regras Especializadas**:
   - Consulte os arquivos de regras relevantes ao escopo da tarefa antes de escrever código.
3. **Verificar Padrões Existentes**:
   - Examine a base de código antes de introduzir novas estruturas para manter consistência absoluta com o estilo já consolidado no projeto.
4. **Reutilização Obrigatória**:
   - Sempre prefira reaproveitar componentes, contratos, validadores, utilitários e tipos já existentes em vez de criar novas versões redundantes.
5. **Nunca Assumir Regras Ambíguas**:
   - Em caso de dúvida sobre regras de negócio, contratos de API, autenticação ou infraestrutura, pergunte e valide com o usuário antes de assumir premissas arbitrárias.
