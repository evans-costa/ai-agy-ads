# Configuração das Pastas de Agentes (`.agents`)

Este repositório reúne as configurações, diretrizes e regras dos agentes de IA, segmentadas por contexto: **Main (Raiz do projeto)**, **Backend** e **Frontend**.

Para utilização no repositório do projeto, cada pasta deve ser posicionada em seu respectivo diretório, **apagando as indicações `(main)`, `(backend)` e `(frontend)`** e mantendo apenas o nome `.agents`.

---

## 1. Mapeamento das Pastas

| Origem (neste repositório) | Destino no Projeto | Nome Final | Descrição |
| :--- | :--- | :--- | :--- |
| `.agents (main)` | Raiz do repositório (`pmv-ads-2026-2-e5-proj-empext-t6-pmv-ads-2026-2-e5-proj-empe-group-2/`) | `.agents` | Diretrizes gerais, briefing, fluxo de Git e `AGENTS.md`. |
| `.agents (backend)` | `codigo-fonte/backend/` | `.agents` | Plugins .NET, arquitetura, testes TDD e regras de backend. |
| `.agents (frontend)` | `codigo-fonte/frontend/` | `.agents` | Design system, formulários, linting, arquitetura e regras de frontend. |

---

## 2. Estrutura Esperada no Projeto

Após a cópia/movimentação, a árvore de diretórios do repositório deve ficar organizada da seguinte forma:

```text
pmv-ads-2026-2-e5-proj-empext-t6-pmv-ads-2026-2-e5-proj-empe-group-2/
│
├── .agents/                               <-- Origem: .agents (main)
│   ├── rules/
│   └── AGENTS.md
│
├── codigo-fonte/
│   ├── backend/
│   │   ├── .agents/                       <-- Origem: .agents (backend)
│   │   │   ├── plugins/
│   │   │   └── rules/
│   │   ├── src/
│   │   └── tests/
│   │
│   └── frontend/
│       ├── .agents/                       <-- Origem: .agents (frontend)
│       │   └── rules/
│       ├── src/
│       └── tests/
│
├── apresentacao/
├── atas/
├── documentos/
└── proex/
```

---

> [!IMPORTANT]
> Em todos os destinos, a pasta deve se chamar estritamente **`.agents`** (com o ponto no início e sem nenhuma indicação entre parênteses) para que as ferramentas e extensões de IA carreguem as regras automaticamente no contexto correto.
