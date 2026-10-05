---
trigger: always_on
description: "Cliente HTTP Axios com interceptors JWT, cache de servidor com TanStack Query e convenções de query keys."
---

# Estado Global e Comunicação com a API - Lessons Hub

> **Descrição**: Consumo da API REST com Axios, cache de servidor com TanStack Query, autenticação JWT e invalidação de cache.

---

## 1. Configuração do Cliente HTTP (Axios)

Instância única localizada em `src/shared/lib/axios.ts`:
- **Base URL**: Definida via `VITE_API_URL` com fallback para o backend local.
- **Headers Padrão**: `Content-Type: application/json`.
- **Interceptor de Requisição**: Injeta `Authorization: Bearer <token>` a partir da sessão.
- **Interceptor de Resposta (401)**: Limpa o armazenamento de autenticação, exibe toast amigável de sessão expirada e redireciona para `/login`.

---

## 2. Gerenciamento de Servidor (TanStack Query)

Configuração global em `src/shared/lib/query-client.ts`:
- `staleTime`: 1 a 2 minutos por padrão para evitar refetching desnecessário.
- `retry`: 1 tentativa em caso de falha de rede.
- `refetchOnWindowFocus`: `false`.

### 2.1 Padrão de Chaves de Consulta (Query Keys)

Utilize sempre **tuplas de arrays** hierárquicas e descritivas:
- `['lessons', 'list', { page, status, search }]`: Listagem com filtros.
- `['lessons', 'detail', lessonId]`: Detalhe de uma lição específica.
- `['kpis', 'double-loop']`: Indicadores do dashboard.
- `['categories', 'list']`: Categorias ativas.
- `['users', 'list', { page }]`: Usuários no painel admin.

---

## 3. Padrão de Hooks em `src/features/<modulo>/api/`

- **Queries (`use*Query`)**: Encapsulam `useQuery` com suas respectivas query keys tipadas e funções de busca.
- **Mutations (`use*Mutation`)**: Encapsulam `useMutation`. No callback `onSuccess`, invalide as queries relacionadas via `queryClient.invalidateQueries({ queryKey: [...] })` e apresente feedback com `toast.success()`.

---

## 4. Estado de Sessão do Usuário

- A sessão do usuário autenticado (token, papel `role`, nome, e-mail) deve ser gerenciada através de um `AuthContext` consumido pelo TanStack Router.
- Nunca persista senhas ou credenciais confidenciais no `localStorage`.
