# Roadmap e pendências

Estado atual e próximos passos. Mantenha este documento sincronizado com a
realidade do repositório.

## Estado atual

- ✅ Plugin funcional: `expectTypeOf()` registra a macro no `TestContext`.
- ✅ Build via `tsdown` + `.d.ts` via `tsc`.
- ✅ CI de commitlint, release automatizado (release-please) e deploy (npm + GitHub Packages).
- ✅ Documentação em `docs/` (esta estrutura).

## Pendências conhecidas

### 1. Cabeçalho de copyright desatualizado em `index.ts`
O arquivo [`index.ts`](../../index.ts) ainda contém:

```ts
/*
 * @japa/expect-type
 *
 * (c) Japa
 * ...
 */
```

Embora o pacote já seja `@t8ngs/expect-type` e importe `@t8ngs/runner`. Deve ser
atualizado para refletir o projeto t8ngs e a autoria correta.

### 2. Ausência de testes automatizados
O script `test` é um placeholder (`echo "No tests"`). Idealmente:

- Adicionar testes de tipo (type-level) que validem o comportamento do plugin.
- Atualizar o script `test`, a [referência de scripts](../references/scripts.md)
  e o [guia de contribuição](../guides/contributing.md).

### 3. Link de "Complete API documentation"
O README original (base `@japa`) apontava para `japa.dev`. Caso o t8ngs tenha um
site de documentação, adicionar o link correspondente no `README.md`.

## Ideias futuras (não comprometidas)

- Exemplos adicionais de matchers no [guia de getting-started](../guides/getting-started.md).
- Workflow de CI que rode `typecheck` e `build` em PRs (hoje a CI foca em commitlint).

## Como registrar novas pendências

Ao identificar uma pendência durante uma alteração, adicione-a aqui **no mesmo
commit**. Agentes de IA devem seguir [`../../AGENT.md`](../../AGENT.md).
