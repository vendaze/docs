# AGENTS.md — vendaze-docs

Este arquivo governa o trabalho de documentação no repositório `vendaze-docs`.
Este repositório é um **submodulo git** do `vendaze-api`, montado em `/docs`.

---

## Contexto do projeto

Este é o site de documentação pública da API do Vendaze, construído com **Mintlify**.

- **Plataforma:** Mintlify
- **Configuração central:** `docs.json` — navegação, branding, temas
- **Páginas:** arquivos `.mdx` com frontmatter YAML
- **API Reference:** gerada automaticamente a partir de `api-reference/openapi.json`
- **Preview local:** `mint dev` (porta 3000)
- **Verificar links:** `mint broken-links`
- **Validar OpenAPI:** `mint validate`

---

## Separação de responsabilidades

| Repositório                            | Conteúdo                                                          |
| -------------------------------------- | ----------------------------------------------------------------- |
| `vendaze-api` (root)                   | Código do Cloudflare Worker — TypeScript, Hono, lógica de negócio |
| `vendaze-docs` (este repo, em `/docs`) | Documentação pública da API — MDX, OpenAPI spec                   |

**Nunca misturar:** regras de código do Worker não se aplicam aqui. Este repo é exclusivamente documentação.

---

## Estrutura de arquivos

```
docs/
├── docs.json                     # Configuração central — navegação, cores, logo
├── favicon.svg                   # Ícone do site
├── index.mdx                     # Página inicial (home)
├── logo/
│   ├── light.svg                 # Logo para tema claro
│   └── dark.svg                  # Logo para tema escuro
├── introduction/                 # Guias de introdução
│   ├── authentication.mdx        # Como funciona o OAuth 2.1
│   ├── register-app.mdx          # Como registrar um app integrador
│   ├── first-request.mdx         # Primeiro request passo a passo
│   └── errors.mdx                # Erros, rate limits, status codes
├── api-reference/
│   ├── openapi.json              # Spec OpenAPI 3.1 completa da API
│   └── introduction.mdx          # Página introdutória da referência
└── snippets/                     # Trechos MDX reutilizáveis
```

---

## Regras de documentação

### Idioma

A documentação é escrita em **inglês**. A API é pública e destinada a integradores globais.

### Tom e voz

- Segunda pessoa: "you", "your app"
- Ativo: "Call this endpoint" não "This endpoint should be called"
- Direto: sem rodeios, sem marketing
- Sentença case nos títulos: "Register your app" não "Register Your App"

### Frontmatter obrigatório em toda página

```yaml
---
title: 'Título da página'
description: 'Uma linha descrevendo o conteúdo.'
icon: 'nome-do-icone-fontawesome'
---
```

### Componentes Mintlify disponíveis

- `<Card>`, `<CardGroup>`, `<Columns>` — navegação e layout
- `<Note>`, `<Warning>`, `<Tip>`, `<Info>` — callouts
- `<Accordion>`, `<AccordionGroup>` — seções colapsáveis
- `<Tabs>`, `<Tab>` — conteúdo alternativo
- `<Steps>` — sequência numerada de passos
- `<CodeGroup>` — múltiplos exemplos de código

### Blocos de código

Sempre incluir a linguagem:

````
```bash
curl ...
```

```json
{ ... }
```
````

### Exemplos de request/response

Todo endpoint de API deve ter:

1. Exemplo de request em `curl`
2. Exemplo de response em `json`
3. Tabela de parâmetros com tipo e descrição

---

## docs.json — regras de atualização

Toda nova página `.mdx` criada **deve** ser adicionada ao `docs.json` na seção correta de `navigation`. Página criada mas não registrada no `docs.json` não aparece no site.

Estrutura de navegação atual:

```json
{
  "navigation": {
    "tabs": [
      { "tab": "Guides", "groups": [...] },
      { "tab": "API Reference", "openapi": "api-reference/openapi.json" }
    ]
  }
}
```

A tab "API Reference" é gerada automaticamente a partir do `openapi.json` — não adicionar páginas manualmente nessa tab.

---

## OpenAPI spec — regras

Arquivo: `api-reference/openapi.json`

- Versão: OpenAPI **3.1.0**
- Base URL: `https://api.vendaze.com`
- Autenticação: Bearer token (OAuth 2.1)
- Todos os endpoints seguem o envelope de resposta padrão do Vendaze
- Tags agrupam os endpoints por recurso: `People`, `Companies`, `Deals`, `Tasks`, `Activities`, `Products`, `Tags`, `Lists`, `Custom Fields`, `OAuth`

Quando um endpoint for implementado no Worker:

1. Adicionar a operação no `openapi.json`
2. O Mintlify gera a página automaticamente a partir da tag

---

## O que não fazer

- Nunca criar páginas de referência de endpoint manualmente — usar o `openapi.json`
- Nunca documentar features internas do Vendaze (painel admin, billing, etc.)
- Nunca mencionar a URL do Supabase na documentação
- Nunca expor `workspace_id` como algo que o integrador precisa gerenciar
- Nunca criar página sem registrar no `docs.json`
- Nunca alterar `docs.json` sem verificar se todos os paths das páginas existem
