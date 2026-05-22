# AniVault API

> API REST para gerenciamento de uma coleção pessoal de animes, com pipeline CI/CD completo usando GitHub Actions e documentação publicada automaticamente via GitHub Pages.

---

## Descrição

A **AniVault API** permite cadastrar, consultar, atualizar e remover animes de um catálogo pessoal. Além disso, oferece um sistema de avaliações onde usuários podem registrar notas e comentários para cada título. O contrato é validado automaticamente com **Spectral** e a documentação interativa é gerada com **Redoc** a cada push na branch `main`.

---

## Links

| Recurso | URL |
|---|---|
| Documentação (GitHub Pages) | `https://SEU_USUARIO.github.io/anivault-api` |
| Repositório | `https://github.com/SEU_USUARIO/anivault-api` |
| Pipeline (Actions) | `https://github.com/SEU_USUARIO/anivault-api/actions` |

---

## Endpoints

### Animes

| Método | Rota | Descrição |
|--------|------|-----------|
| `GET` | `/animes` | Lista todos os animes (aceita filtros por `genero` e `status`) |
| `POST` | `/animes` | Cadastra um novo anime no catálogo |
| `GET` | `/animes/{id}` | Busca um anime específico pelo ID |
| `PUT` | `/animes/{id}` | Atualiza os dados de um anime existente |
| `DELETE` | `/animes/{id}` | Remove um anime do catálogo |

### Avaliações

| Método | Rota | Descrição |
|--------|------|-----------|
| `POST` | `/animes/{id}/avaliacoes` | Registra uma avaliação (nota + comentário) para um anime |
| `GET` | `/animes/{id}/avaliacoes` | Lista todas as avaliações de um anime |

---

## Pipeline CI/CD

O arquivo `.github/workflows/ci-cd.yml` define dois jobs em sequência:

```
push para main
     │
     ▼
┌─────────────┐
│   validar   │  ← Spectral lint no openapi.yaml
└──────┬──────┘
       │ needs: validar
       ▼
┌──────────────────┐
│ publicar-docs    │  ← Redoc gera HTML → deploy no GitHub Pages
└──────────────────┘
```

1. **`validar`** — instala o Spectral CLI e roda `spectral lint openapi.yaml` com o ruleset `spectral:oas`
2. **`publicar-docs`** — só executa se o job anterior passar; gera `docs/index.html` com Redoc e faz deploy no GitHub Pages

---

## Como configurar o repositório

### 1. Criar o repositório no GitHub

```bash
git init
git remote add origin https://github.com/SEU_USUARIO/anivault-api.git
```

### 2. Habilitar o GitHub Pages

Acesse: **Settings → Pages → Source → GitHub Actions**

### 3. Fazer o primeiro push

```bash
git add .
git commit -m "feat: contrato OpenAPI e pipeline CI/CD"
git push -u origin main
```

### 4. Verificar o pipeline

Acesse a aba **Actions** do repositório e confirme que os dois jobs passaram com.

---

## Estrutura do projeto

```
anivault-api/
├── openapi.yaml                  # Contrato OpenAPI 3.0.3
├── README.md                     # Este arquivo
└── .github/
    └── workflows/
        └── ci-cd.yml             # Pipeline GitHub Actions
```

---

## Schemas definidos

| Schema | Descrição |
|--------|-----------|
| `Anime` | Objeto completo de anime (resposta) |
| `AnimeInput` | Dados para criar/atualizar anime (request body) |
| `Avaliacao` | Objeto completo de avaliação (resposta) |
| `AvaliacaoInput` | Dados para registrar avaliação (request body) |
| `Erro` | Estrutura padrão de erros da API |
