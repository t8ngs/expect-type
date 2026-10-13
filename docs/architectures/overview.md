# Visão geral da arquitetura

Este documento descreve a arquitetura do `@t8ngs/expect-type`.

## 1. Em uma frase

`@t8ngs/expect-type` é um **plugin** do `@t8ngs/runner` que injeta a função
`expectTypeOf` no **contexto de teste** (`TestContext`), delegando toda a lógica
de asserção de tipos para a biblioteca [`expect-type`](https://www.npmjs.com/package/expect-type).

## 2. Componentes

```
┌─────────────────────────────────────────────────────────────┐
│                      @t8ngs/runner                           │
│                                                              │
│   configure({ plugins: [ expectTypeOf() ] })                 │
│                        │                                     │
│                        ▼                                     │
│        ┌───────────────────────────────┐                    │
│        │  PluginFn (retornado por       │                    │
│        │  expectTypeOf())               │                    │
│        │                                │                    │
│        │  TestContext.macro(            │                    │
│        │    'expectTypeOf', expectType) │                    │
│        └───────────────┬───────────────┘                    │
│                        │ adiciona a macro                    │
│                        ▼                                     │
│        ┌───────────────────────────────┐                    │
│        │  TestContext                   │                    │
│        │   .expectTypeOf  ◄─────────────┼── usado nos testes │
│        └───────────────────────────────┘                    │
└─────────────────────────────────────────────────────────────┘
                         │
                         ▼
             ┌───────────────────────┐
             │  expect-type (npm)    │
             │  expectTypeOf(...)    │
             └───────────────────────┘
```

- **`expectTypeOf()`** (arquivo [`index.ts`](../../index.ts)): função fábrica que
  retorna um `PluginFn`.
- **`PluginFn`**: ao ser executado pelo runner, registra uma **macro** chamada
  `expectTypeOf` no `TestContext`.
- **`TestContext.macro(...)`**: mecanismo do `@t8ngs/runner` para estender o
  contexto de teste com novas propriedades/métodos.
- **`expect-type`**: biblioteca externa que fornece a implementação real das
  asserções de tipo.

## 3. Fluxo de execução

1. O usuário registra o plugin em `configure({ plugins: [expectTypeOf()] })`.
2. O runner executa o `PluginFn`, que chama
   `TestContext.macro('expectTypeOf', expectType)`.
3. A partir daí, todo teste recebe `expectTypeOf` no seu contexto
   (`({ expectTypeOf }) => { ... }`).
4. As asserções são avaliadas pelo **compilador TypeScript**, não em runtime.

## 4. Augmentação de módulo (type-level)

O `index.ts` declara uma **module augmentation**:

```ts
declare module '@t8ngs/runner/core' {
  interface TestContext {
    expectTypeOf: typeof expectType
  }
}
```

Isso garante que o TypeScript "enxergue" a propriedade `expectTypeOf` no
`TestContext`, dando autocomplete e checagem de tipos corretos nos testes.

## 5. Sem comportamento em runtime

A característica central deste pacote: **as asserções não falham em tempo de
execução**. Elas são verificadas durante a **compilação**. Portanto:

- Erros de asserção aparecem ao rodar `npm run typecheck` ou `tsc`.
- Em runtime, `expectTypeOf(...)` é essencialmente um no-op.

## 6. Estratégia de build

Definida em `package.json` (campo `tsdown`) e `tsconfig.json`:

- **Bundler:** [`tsdown`](https://tsdown.dev) — gera o JS em formato ESM na pasta `build/`.
- **Tipos:** `tsc --emitDeclarationOnly --declaration` gera os `.d.ts`.
- **Entry:** `index.ts`.
- **Formato:** ESM (`"type": "module"`).
- **Saída publicada:** campo `exports` aponta para `./build/index.js`.

Ver detalhes em [referência de configurações](../references/configuration.md) e
[guia de build e publicação](../guides/build-and-release.md).

## 7. Dependências relevantes

- **`expect-type`** (dependency): implementação das asserções.
- **`@t8ngs/runner`** (peerDependency): o runner que hospeda o plugin.

## Observação de manutenção

> O cabeçalho de copyright em `index.ts` ainda menciona `@japa/expect-type` /
> `(c) Japa`, embora o código já utilize `@t8ngs/runner`. Ver pendência em
> [`../planning/roadmap.md`](../planning/roadmap.md).
