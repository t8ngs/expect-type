# Visão geral do projeto

## Identidade

| Campo | Valor |
|-------|-------|
| Nome | `@t8ngs/expect-type` |
| Descrição | Write assertions for TypeScript types |
| Versão | `1.0.0` |
| Licença | MIT |
| Autor | Jefte Costa `<jefteamorim@gmail.com>` |
| Repositório | https://github.com/t8ngs/expect-type |
| Tipo de módulo | ESM (`"type": "module"`) |
| Node mínimo | `>= 18.16.0` |

## O que é

Plugin do ecossistema **t8ngs** que adiciona asserções de **tipos TypeScript**
aos testes executados pelo `@t8ngs/runner`. É uma camada fina sobre a biblioteca
[`expect-type`](https://www.npmjs.com/package/expect-type).

As asserções são verificadas em **tempo de compilação** (não em runtime).

## Ecossistema e relações

```
@t8ngs/runner  ──(peerDependency)──►  @t8ngs/expect-type  ──(dependency)──►  expect-type
```

- **Depende de:** `expect-type`.
- **Peer dependency:** `@t8ngs/runner` (`^1.0.1`) — o usuário final deve tê-lo.
- **Pertence a:** organização/escopo `@t8ngs`.

## Estrutura de pastas (alto nível)

```
expect-type/
├── index.ts                 # Código-fonte (única fonte da API pública)
├── package.json             # Metadados, scripts, deps, config de build
├── tsconfig.json            # Config TypeScript (estende @arapucajs/tsconfig)
├── eslint.config.js         # Config ESLint (@adonisjs/eslint-config)
├── commitlint.config.js     # Conventional Commits
├── .editorconfig            # Estilo de edição
├── .npmrc                   # package-lock=false
├── .prettierignore          # Ignora build, docs, *.html
├── LICENSE                  # MIT
├── README.md                # Visão rápida do pacote
├── .husky/                  # Git hooks (pre-commit, commit-msg)
├── .github/workflows/       # CI/CD (checks, release, deploy, stale, labels)
├── build/                   # Saída gerada (não versionada)
└── docs/                    # Documentação (esta pasta)
    ├── README.md            # Índice central
    ├── guides/              # Guias práticos
    ├── references/          # Referência técnica
    ├── architectures/       # Arquitetura
    ├── planning/            # Roadmap e pendências
    └── projects/            # Visão geral (este documento)
```

## Público-alvo

- **Humanos:** desenvolvedores que usam `@t8ngs/runner` e querem validar tipos.
- **Agentes de IA:** devem ler [`../../AGENT.md`](../../AGENT.md) e
  [`../../CLAUDE.md`](../../CLAUDE.md) antes de alterar o projeto.

## Documentos relacionados

- [Arquitetura](../architectures/overview.md)
- [API](../references/api.md)
- [Roadmap](../planning/roadmap.md)
