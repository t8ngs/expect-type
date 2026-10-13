# Build e publicação

Como buildar localmente e como a publicação automatizada funciona.

## Build local

```sh
npm run build
```

Esse comando executa `compile`:

1. `tsdown` — gera o JS em formato ESM na pasta `build/`.
2. `tsc --emitDeclarationOnly --declaration` — gera os arquivos de tipos `.d.ts`.

> O hook `precompile` roda `npm run lint` antes de compilar.

Para limpar a saída:

```sh
npm run clean   # remove build/
```

## O que é publicado

Definido em `package.json`:

- `files`: `build` (exceto `build/bin` e `build/tests`).
- `exports`: `"." → "./build/index.js"`.
- `prepublishOnly`: roda `npm run build` antes de qualquer publicação.

## Fluxo de release automatizado

O projeto usa **release-please** + GitHub Actions. O fluxo é totalmente dirigido
por Conventional Commits.

```
push na main
   │
   ▼
release.yml (release-please)
   │  cria/atualiza um "release PR" com changelog e bump de versão
   ▼
merge do release PR
   │  cria a tag e a GitHub Release; instala dependências, gera build/ e faz upload do .tgz
   ▼
release publicado  ──►  deploy.yml
                          ├─ publica no GitHub Packages
                          └─ publica no npm (com --provenance)
```

### Workflows envolvidos

| Workflow | Gatilho | Papel |
|----------|---------|-------|
| [`release.yml`](../../.github/workflows/release.yml) | push em `main` | Cria o release PR (release-please) e, ao liberar, sobe o artefato `.tgz`. |
| [`deploy.yml`](../../.github/workflows/deploy.yml) | `release` publicado | Publica o pacote no GitHub Packages e no npm. |

O workflow [`checks.yml`](../../.github/workflows/checks.yml) valida os commits
com commitlint. Ele não usa o cache npm do `setup-node`, pois o repositório não
gera lockfile e o cache requer um lockfile.

O workflow de release instala dependências com `npm install` (não há lockfile,
por política do `.npmrc`) e só executa a instalação e o build quando uma nova
release foi criada. O deploy também usa `npm install` nos dois jobs, pois `npm ci`
exige um lockfile. No job do GitHub Packages, o escopo `@t8ngs` é direcionado ao
npm público durante a instalação para que dependências públicas desse escopo não
exijam autenticação do GitHub Packages; a publicação do pacote continua usando
o registry do GitHub Packages. O build gera `build/` antes de `npm pack`
empacotar o artefato.

### Secrets necessários

- `RELEASE_PLEASE_TOKEN` — usado pelo release-please.
- `NPM_TOKEN` — publicação no npm.
- `GITHUB_TOKEN` — fornecido pelo GitHub Actions (publicação no GitHub Packages e upload do artefato).

## Versionamento

- A versão é derivada dos commits (SemVer via Conventional Commits).
- O script `version` roda `npm run build` ao versionar.

## Ao mudar o processo de build/release

> Atualize este guia e a [referência de configurações](../references/configuration.md)
> no mesmo commit.
