---
trigger: always_on
description: "Fonte canônica de tokens visuais, cores, tipografia, espaçamentos, componentes e assets do Design System."
---

# LessonsHub — Design Spec & Design System

> **Fonte Canônica da Verdade**: Tokens visuais, tipografia, espaçamentos, componentes e assets extraídos do Figma (`O5getUO7rL5ZgVsy1tOjMn` e `documentos/03-Projeto de Interface.md`). Use variáveis de `styles.css` e classes canônicas do Tailwind v4. Proibido medidas arbitrárias em `[...px]`.

---

## 1. Cores e Tokens de Tema

### 1.1 Paleta Principal (`src/styles.css` @theme / :root)

| Token CSS / Tailwind | Hex | Uso Obrigatório |
|---|---|---|
| `--color-brand-blue` | `#0026FF` | Painel de login, fundo decorativo |
| `--color-brand-blue2` | `#0055FF` | Botão primário, links ativos, paginação ativa (hover: `#0043CC`, active: `#0036A3`) |
| `--color-danger` | `#FF2F52` | Borda/ring/texto de erro, hover botão cancelar, logout (active/banner: `#CB0A2B`) |
| `--color-success-dark` | `#008C5E` | Active botão salvar rascunho |
| `--color-surface-neutral` | `#F8F9FA` | Fundo th de tabela, inputs disabled, hover linhas/paginação |
| `--color-step-inactive` | `#6F6F6B` | Borda, texto e linha conectora dos passos pendentes do Wizard |
| `--color-text-primary` | `#111111` | Títulos, labels de campos, texto principal |
| `--color-text-dark` | `#1E293B` | Títulos de modais |
| `--color-text-secondary` | `#6C6C68` | Subtítulos, descrições, células secundárias |
| `--color-text-placeholder` | `#A3A3A0` | Placeholder de inputs e selects |
| `--color-text-muted` | `#9A9A95` | Cargo no perfil de usuário, stroke de ícones da tabela |
| `--color-border` | `#DEDEDA` | Bordas padrão de inputs, modais e botões secundários |
| `--color-white` | `#FFFFFF` | Fundo de página, painéis, modais, inputs |

### 1.2 Badges de Status de Lição (`rounded-full px-3 py-1.5 text-xs font-medium`)

| Status | Background & Texto | Hex Referência | Descrição |
|---|---|---|---|
| **Ativo** | `bg-green-100 text-green-800` | `#DCFCE7` / `#166534` | Lição publicada e ativa |
| **Inativo / Arquivado** | `bg-slate-100 text-slate-600` | `#F1F5F9` / `#475569` | Lição arquivada ou desativada |
| **Rascunho** | `bg-yellow-100 text-yellow-900` | `#FEF9C3` / `#713F12` | Lição em elaboração (Draft) |
| **Pendente** | `bg-amber-100 text-amber-800` | `#FEF3C7` / `#92400E` | Aguardando execução/validação de ações |
| **Institucionalizada** | `bg-brand-blue2 text-white` | `#0055FF` / `#FFFFFF` | Ciclo Double-Loop fechado com evidências |

### 1.3 Tags de Categoria & Status Bullets

- **Tags (`rounded-full px-2 py-1 text-xs font-medium`, prefixo `#`)**:
  - `#fornecedores`: `bg-blue-50 text-blue-700` (`#EFF6FF` / `#1D4ED8`) · `#implantação`: `bg-green-50 text-green-700` (`#F0FDF4` / `#15803D`)
  - `#SLA`: `bg-purple-50 text-purple-700` (`#FAF5FF` / `#7E22CE`) · `#documentação`: `bg-orange-50 text-orange-700` (`#FFF7ED` / `#C2410C`)
  - `#planejamento`: `bg-yellow-50 text-yellow-700` (`#FEFCE8` / `#A16207`) · Fallback: `bg-slate-100 text-slate-600` (`#F1F5F9` / `#475569`)
- **Status Bullet (`size-9 rounded-md` com ícone SVG centralizado `size-5.25`)**:
  - Pending: `bg-yellow-50` (`#FEFCE8`) + `assets/53cbc.svg`
  - Ok: `bg-blue-50` (`#EFF6FF`) + `assets/1dce3.svg`
  - Status3: `bg-surface-neutral` (`#F8F9FA`) + `assets/314ba.svg`

---

## 2. Tipografia (`Inter, sans-serif`)

| Estilo | Tamanho / Peso | Classes Tailwind | Uso Principal |
|---|---|---|---|
| **Page Title** | 32px / 600 SemiBold | `text-3xl font-semibold` | Título de login ("Bem-vindo.") |
| **Section Title** | 24px / 700 Bold | `text-2xl font-bold` | "Listagem de Lições", "Painel de Administração" |
| **Card / Modal Title** | 20px / 700 Bold | `text-xl font-bold` | Títulos de modais ("Cadastro de Usuário") |
| **Subsection Title** | 18px / 700 Bold | `text-lg font-bold` | Títulos das etapas do formulário multi-step |
| **Label / Button** | 14px / 600 SemiBold | `text-sm font-semibold` | Labels de formulário, botões, abas, th de tabela |
| **Body** | 14px / 400 Regular (1.5) | `text-sm font-normal leading-relaxed` | Parágrafos, descrições, células de tabela |
| **Caption / Placeholder** | 14px / 400 Regular | `text-sm font-normal` | Placeholders de campos, textos de apoio |
| **Tag / Badge** | 12px / 500 Medium | `text-xs font-medium` | Tags de categoria, badges de status |

---

## 3. Espaçamentos, Raios e Bordas (Proibido `[...px]`)

> **Regra Obrigatória**: Divida pixels por 4 para utilizar a escala nativa do Tailwind (ex: `360px` -> `sm:w-90`, `280px` -> `w-70`, `48px` -> `h-12`, `36px` -> `h-9`). Proibido usar valores arbitrários entre colchetes em pixels.

- **Espaçamentos**:
  - `8px`: `gap-2`, `p-2` · `12px`: `py-3`, `gap-3` · `16px`: `px-4`, `p-4` · `20px`: `px-5`, `gap-5` · `22px`: `gap-5.5`
  - `28px`: `gap-7` · `30px`: `gap-7.5` · `32px`: `p-8` · `40px`: `p-10`, `gap-10` · `50px`: `gap-12.5`
- **Raios de Borda (`border-radius`)**:
  - `rounded-md` (6px): paginação, container de status bullet
  - `rounded-lg` (8px): inputs, selects, botões Cancelar e Salvar Rascunho
  - `rounded-[10px]` (10px): botão primário (`bg-brand-blue2`), menu aberto de User Dropdown
  - `rounded-2xl` (16px): painel de modais, botão fechar modal (×)
  - `rounded-[20px]` (20px): cartões de login (azul e branco)
  - `rounded-full` (9999px): avatares, tags de categoria, badges de status, campo de busca
- **Bordas**:
  - `1px solid`: `border border-border` (`#DEDEDA` - inputs, modais, botão secundário) · `border border-neutral-200` (`#E5E5E5` - paginação inativa, dropdown)
  - `2px solid`: `border-2 border-brand-blue2` (`#0055FF` - aba ativa admin, step conectado) · `border-2 border-step-inactive` (`#6F6F6B` - step pendente)

---

## 4. Especificações de Componentes

### 4.1 Botões

| Botão | Formato / Base | Default | Hover | Active |
|---|---|---|---|---|
| **Primário** | `px-6 py-3 rounded-[10px] text-sm font-semibold` | `bg-brand-blue2 text-white` | `hover:bg-brand-blue2-hover` | `active:bg-brand-blue2-active` |
| **Cancelar** | `h-12 px-6 rounded-lg text-xs font-normal border` | `bg-white border-border text-text-primary` | `hover:bg-danger hover:border-danger hover:text-white` | `active:bg-danger-dark active:border-danger-dark active:text-white` |
| **Salvar Rascunho** | `h-12 px-6 rounded-lg text-xs font-normal border` | `bg-white border-border text-text-primary` | `hover:bg-emerald-500 hover:border-emerald-500 hover:text-white` | `active:bg-success-dark active:border-success-dark active:text-white` |

- *Labels do Primário*: `Login`, `Salvar`, `Continuar →`, `Finalizar →`, `+ Nova Lição`, `Gera Institucionalização`.

### 4.2 Formulários & Step Indicator

- **Input & Textarea**: `bg-white border border-border rounded-lg px-4 py-3.5 text-sm text-text-primary placeholder:text-text-placeholder disabled:bg-surface-neutral`. Erro: `border-danger focus-visible:ring-danger text-danger`.
- **Select**: `h-12 pr-10 pl-4 bg-white border border-border rounded-lg text-sm text-text-primary disabled:bg-surface-neutral` + chevron down `size-4 text-text-secondary`.
- **Step Indicator (Wizard 3 Etapas)**:
  - Círculo: `size-10 rounded-full flex items-center justify-center`. Linha: `h-[1.25px]`.
  - *Completo*: Círculo `bg-brand-blue2 text-white` com checkmark `assets/b8e20.svg` (24×24px). Linha saída: `bg-brand-blue2`. Título: `text-sm font-semibold text-text-primary`. Subtítulo: `text-xs text-text-secondary`.
  - *Ativo*: Círculo `bg-brand-blue2 text-white text-base font-medium`. Linha saída: `bg-step-inactive`. Título: `text-sm font-semibold text-text-primary`.
  - *Pendente*: Círculo `border-2 border-step-inactive text-step-inactive text-base font-medium`. Linha saída: `bg-step-inactive`. Título: `text-sm font-medium text-step-inactive`.

### 4.3 Busca, Abas, Tabela e Paginação

- **Busca (Header)**: `bg-slate-100 rounded-full py-2 pr-4 pl-11 text-sm placeholder:text-text-secondary border-none` + Search `size-4 text-text-secondary`.
- **Abas Admin**: `text-sm font-semibold border-b border-border`. Aba ativa: `text-brand-blue2 border-b-2 border-brand-blue2`. Aba inativa: `text-text-secondary`. (`Usuários`, `Projetos`, `Lições`, `Categorias`).
- **Tabela**:
  - Header: `bg-surface-neutral text-[11px] font-semibold uppercase tracking-wider text-text-secondary px-4 py-3 border-b border-border`. (`LIÇÃO`, `CATEGORIA`, `PROJETO`, `DATA`, `STATUS`, `AÇÕES`).
  - Linha: `bg-white hover:bg-surface-neutral/60 px-4 py-3 border-b border-border`. Título: `text-sm font-semibold text-text-primary`. Corpo: `text-sm text-text-secondary`.
  - Ações: botões com `hover:bg-slate-100 p-1 rounded`, ícones editar (`assets/a49e0.svg`, 16×16px) e excluir (`assets/961d8.svg`, 16×16px), stroke `text-text-muted`.
- **Paginação**: Botões `size-8 rounded-md text-sm gap-2`. Ativo: `bg-brand-blue2 text-white font-semibold`. Inativo: `bg-white border border-neutral-200 hover:bg-surface-neutral text-text-primary font-medium`. Contagem: `text-sm text-text-secondary`.

### 4.4 Usuário & Modal

- **Avatar**: `rounded-full`, 37px (header) / `size-8` (tabela). Iniciais `text-xs font-bold text-white`: EC (`bg-indigo-500`), AS (`bg-emerald-500`), CM (`bg-amber-500`), JF (`bg-blue-500`), RS (`bg-red-500`), fallbacks (`bg-violet-500`, `bg-pink-500`, `bg-teal-500`). Placeholder: `bg-slate-100`.
- **User Dropdown**:
  - Fechado: Avatar 37px (`assets/c01eb.png`), nome `text-xs font-semibold text-text-primary`, cargo `text-xs font-semibold text-text-muted`, chevron `assets/09fdf.svg` (12×6px), container `hover:bg-surface-neutral p-2 rounded-lg`.
  - Aberto: Painel `bg-white rounded-[10px] border border-neutral-200 shadow-md p-1`. Separador: `border-b border-neutral-200`. Item: `text-sm font-semibold text-text-secondary hover:bg-surface-neutral hover:text-text-primary px-3 py-2 rounded`. Item logout: `text-sm font-semibold text-danger hover:bg-red-50 px-3 py-2 rounded`. Chevron: `assets/27c6b.svg`.
- **Modal**:
  - Painel: `bg-white rounded-2xl p-8 gap-7 shadow-xl max-w-lg w-full`.
  - Header: Título `text-xl font-bold text-text-dark`, botão fechar `size-8 rounded-full hover:bg-surface-neutral flex items-center justify-center` com ícone × `size-3.5`.
  - Footer: `flex items-center justify-between pt-4` com botão Cancelar (§ 4.1) e botão Primário Confirmar (§ 4.1).

---

## 5. Ícones e Assets

### 5.1 Assets Locais (`src/assets/`)

| Arquivo | Tipo | Dimensões | Uso Obrigatório |
|---|---|---|---|
| `assets/logo.png` | PNG | 181×32px (ratio 711:125) | Logo LessonsHub no header principal |
| `assets/b8e20.svg` | SVG | 24×24px | Checkmark branco para step indicator completo |
| `assets/a49e0.svg` | SVG | 16×16px | Ícone lápis (ação editar na tabela) |
| `assets/961d8.svg` | SVG | 16×16px | Ícone lixeira (ação excluir na tabela) |
| `assets/53cbc.svg` | SVG | 21×21px | Status bullet Pending (amarelo) |
| `assets/1dce3.svg` | SVG | 21×21px | Status bullet Ok (azul) |
| `assets/314ba.svg` | SVG | 21×21px | Status bullet Status3 (cinza) |
| `assets/09fdf.svg` | SVG | 12×6px | Chevron down no User Dropdown |
| `assets/27c6b.svg` | SVG | 12×6px | Chevron up no User Dropdown aberto |
| `assets/ec4ea.svg` | SVG | 1.25px alt | Linha conectora azul ativa (`#0055FF`) no Wizard |
| `assets/53069.svg` | SVG | 1.25px alt | Linha conectora cinza pendente (`#6F6F6B`) no Wizard |

### 5.2 Ícones Auxiliares (`lucide-react`)

Para ícones complementares (`Search`, `LogOut`, `AlertCircle`, `Calendar`), use `lucide-react` com stroke `1.5px` ou `2px` e tamanhos canônicos `size-4` (16px), `size-5` (20px) ou `size-6` (24px).
