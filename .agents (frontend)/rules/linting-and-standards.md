---
trigger: always_on
description: "Padrões estritos de qualidade de código Ultracite/Biome, TypeScript, React 19, regras Tailwind e acessibilidade."
---

# Padrões de Código e Linting (Ultracite) - Frontend Lessons Hub

> **Descrição**: Diretrizes estritas de qualidade baseadas no motor Biome/Ultracite. O código deve ser acessível, performático, type-safe e autoexplicativo.

---

## 1. Comandos CLI Ultracite

| Ação | Atalho npm | Comando Direto |
|---|---|---|
| **Verificar inconsistências** | `npm run lint` | `npx ultracite check` |
| **Corrigir e formatar automaticamente** | `npm run fix` | `npx ultracite fix` |
| **Diagnosticar ambiente** | — | `npx ultracite doctor` |

---

## 2. Regras Mandatórias e Inegociáveis

1. **Zero Supressões de Linter**: **NUNCA** utilize comentários de supressão (`// biome-ignore ...`). Todo alerta de linter/formatação deve ser resolvido por refatoração limpa no código.
2. **Proibido Modificar `biome.jsonc`**: Nunca altere `biome.jsonc` de forma autônoma. Qualquer mudança requer autorização prévia explícita do usuário.
3. **Proibido Barrel Files (`noBarrelFile`)**: Não crie arquivos `index.ts` que reexportam múltiplos módulos (`src/components/ui/index.ts`). Faça importações diretas do arquivo do componente (ex: `import { Button } from '@/components/ui/Button'`).
4. **Sem Bind / Inline Arrow em Props JSX (`noJsxPropsBind`)**: Nunca repasse arrow functions inline diretamente em callbacks JSX (`onClick={() => handleClick(id)}`). Envolva manipuladores com `useCallback` ou extraia subcomponentes dedicados que recebam os identificadores via props.
5. **Proibido Comentários no Código**: Nunca insira comentários explicativos, redundantes, óbvios ou de seção. O código deve ser 100% autoexplicativo através de tipagem estrita, nomenclatura semântica e clareza arquitetural.
6. **Proibido Classes Arbitrárias em Pixels (`[...px]`)**: Nunca use medidas literais entre colchetes (ex: `sm:w-[360px]`, `gap-[22px]`, `h-[36px]`). Divida os pixels por 4 e use a escala nativa do Tailwind (ex: `sm:w-90`, `gap-5.5`, `h-9`, `w-70`, `h-12`).
7. **Uso Estrito de Variáveis de Tema (`styles.css`)**: Use sempre variáveis do Tailwind registradas no `@theme` (`bg-brand-blue2`, `border-border`, `text-text-primary`, `text-text-secondary`, `text-danger`) em vez de hexadecimais arbitrários.
8. **Classes Canônicas do Tailwind**: Utilize sempre utilitários canônicos compactos (ex: `size-4`, `size-8`, `size-full`, `rounded-full`) em vez de pares redundantes como `w-4 h-4`.

---

## 3. Diretrizes de Qualidade Técnica

### 3.1 TypeScript Estrito
- **Sem `any`**: Utilize `unknown` com *type narrowing* (`typeof`, `in`, `instanceof`) quando o tipo for indeterminado.
- **Tipagem Explícita**: Tipagem explícita para parâmetros de função, interfaces de props e retornos de queries/mutations.
- **Asserções Constantes**: Utilize `as const` para objetos e arrays imutáveis e tipos literais.
- **Sem Números Mágicos**: Extraia números e strings constantes em identificadores descritivos.

### 3.2 React 19 & Arquitetura de Componentes
- **Componentes Funcionais Puros**: Funções declarativas ou arrow functions puras.
- **React 19 - `ref` como Prop Direta**: Repasse `ref` diretamente como prop funcional comum, dispensando `React.forwardRef`.
- **Regras dos Hooks**: Chame hooks exclusivamente no nível raiz do componente. Mantenha arrays de dependências de `useEffect`, `useCallback` e `useMemo` rigorosamente exaustivos.
- **Prop `key` Estável**: Sempre forneça `key` estável e única (IDs de entidade; nunca índices de array).
- **Early Returns**: Retorne precocemente em condições de guarda e validação para evitar aninhamento desnecessário.
- **Sem Código de Debug**: Remova `console.log`, `debugger` ou alertas antes de commits.

### 3.3 Acessibilidade (a11y) & Segurança
- **Elementos Semânticos**: Utilize tags semânticas HTML5 (`<button>`, `<nav>`, `<header>`, `<main>`, `<aside>`) em vez de `<div>` com manipuladores de clique.
- **Labels e Acessibilidade por Teclado**: Todo input deve ter label associado (`htmlFor` ou `aria-label`). Elementos interativos devem responder a `Enter` e `Espaço`.
- **Links Externos Seguros**: Utilize `rel="noopener noreferrer"` em links com `target="_blank"`.
- **Sanitização**: Valide todas as entradas de dados e payloads via schemas Zod. Proibido `eval()` ou `dangerouslySetInnerHTML`.
