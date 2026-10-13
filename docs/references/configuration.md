# Referência de configurações

Mapa de todos os arquivos de configuração do projeto e o que cada um controla.

## TypeScript — [`tsconfig.json`](../../tsconfig.json)

```json
{
  "extends": "@arapucajs/tsconfig/tsconfig.package.json",
  "compilerOptions": {
    "rootDir": "./",
    "outDir": "./build"
  }
}
```

- Estende o preset `@arapucajs/tsconfig` (variante para pacotes).
- `rootDir` na raiz, saída em `build/`.

## Build — campo `tsdown` do [`package.json`](../../package.json)

| Opção | Valor | Significado |
|-------|-------|-------------|
| `entry` | `["index.ts"]` | Ponto de entrada. |
| `outDir` | `./build` | Pasta de saída. |
| `clean` | `true` | Limpa a saída antes de buildar. |
| `format` | `esm` | Gera módulos ES. |
| `minify` | `dce-only` | Apenas dead-code elimination. |
| `dts` | `false` | Os `.d.ts` são gerados pelo `tsc`, não pelo tsdown. |
| `treeshake` | `false` | Tree-shaking desativado. |
| `sourcemaps` | `false` | Sem source maps. |
| `target` | `esnext` | Alvo moderno. |

## Empacotamento npm — [`package.json`](../../package.json)

- `"type": "module"` → o pacote é ESM.
- `exports`: `"." → "./build/index.js"`.
- `files`: publica `build`, exceto `build/bin` e `build/tests`.
- `engines.node`: `>=18.16.0`.
- `dependencies`: `expect-type`.
- `peerDependencies`: `@t8ngs/runner` (`^1.0.1`).
- `publishConfig`: acesso público + provenance.

## ESLint — [`eslint.config.js`](../../eslint.config.js)

```js
import { configPkg } from '@adonisjs/eslint-config'
export default configPkg({ ignores: ['coverage'] })
```

Usa o preset de pacote do `@adonisjs/eslint-config`, ignorando `coverage/`.

## Prettier

- Preset: `"prettier": "@arapucajs/prettier"` (no `package.json`).
- Ignorados: [`.prettierignore`](../../.prettierignore) → `build`, `docs`, `*.html`.

## commitlint — [`commitlint.config.js`](../../commitlint.config.js)

```js
export default { extends: ['@commitlint/config-conventional'] }
```

Exige **Conventional Commits** (`feat:`, `fix:`, `docs:`, etc.).

## Git hooks — [`.husky/`](../../.husky/)

| Hook | Conteúdo | Efeito |
|------|----------|--------|
| `pre-commit` | `npm test` | Roda `pretest` (lint) + `test` antes de cada commit. |
| `commit-msg` | `npx --no -- commitlint --edit $1` | Valida a mensagem de commit. |

## EditorConfig — [`.editorconfig`](../../.editorconfig)

- Indentação: 2 espaços, LF, UTF-8, remove espaços finais, newline final.
- `*.md`: não remove espaços finais (preserva quebras de linha Markdown).

## npm — [`.npmrc`](../../.npmrc)

- `package-lock=false` → não gera `package-lock.json`.

## CI/CD — [`.github/workflows/`](../../.github/workflows/)

| Workflow | Gatilho | O que faz |
|----------|---------|-----------|
| `checks.yml` | push/PR em `main` | Valida commits com commitlint. |
| `release.yml` | push em `main` | `release-please` cria PR de release e artefato `.tgz`. |
| `deploy.yml` | release publicado | Publica no **GitHub Packages** e no **npm** (com provenance). |
| `stale.yml` | cron diário | Marca/fecha issues e PRs inativos. |
| `labels.yml` | manual (`workflow_dispatch`) | Sincroniza labels do repositório. |

## Ao alterar qualquer config

> Atualize este documento **no mesmo commit** da mudança. Ver [`../../AGENT.md`](../../AGENT.md).
