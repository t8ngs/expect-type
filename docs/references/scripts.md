
# Referência de scripts npm

Fonte: campo `scripts` do [`package.json`](../../package.json).

| Script | Comando | O que faz |
|--------|---------|-----------|
| `test` | `echo "No tests"` | **Placeholder.** Ainda não há suíte de testes (ver [roadmap](../planning/roadmap.md)). |
| `pretest` | `npm run lint` | Executa automaticamente antes de `test`: roda o ESLint. |
| `lint` | `eslint .` | Analisa o código com ESLint. |
| `format` | `prettier --write .` | Formata todos os arquivos com Prettier. |
| `typecheck` | `tsc --noEmit` | Checa tipos (inclui as asserções de tipo) sem gerar arquivos. |
| `clean` | `del-cli build` | Remove a pasta `build/`. |
| `compile` | `tsdown && tsc --emitDeclarationOnly --declaration` | Gera o JS (ESM) e os `.d.ts`. |
| `precompile` | `npm run lint` | Executa automaticamente antes de `compile`. |
| `build` | `npm run compile` | Alias de `compile`; é o comando de build oficial. |
| `version` | `npm run build` | Executado pelo `npm version`: rebuilda antes de versionar. |
| `prepublishOnly` | `npm run build` | Garante build antes de publicar no npm. |
| `prepare` | `husky` | Instala os git hooks do husky após `npm install`. |
| `commitlint` | `commitlint --edit` | Valida a mensagem de commit. |
| `commit` | `cz` | Abre o assistente de commit (commitizen / conventional). |

## Comandos mais usados no dia a dia

```sh
npm run typecheck   # valida tipos e asserções
npm run lint        # checa estilo/qualidade
npm run format      # formata o código
npm run build       # gera build/ (JS + .d.ts)
npm run clean       # limpa build/
```

## Observações

- Não há `postinstall` que gere build; a publicação usa `prepublishOnly`.
- O script `test` é intencionalmente um placeholder. Ao adicionar testes reais,
  **atualize este documento** e o [roadmap](../planning/roadmap.md).
