# Referência da API

Fonte: [`index.ts`](../../index.ts). O pacote expõe **uma única** API pública.

## `expectTypeOf()`

```ts
export function expectTypeOf(): PluginFn
```

- **Tipo:** função fábrica (factory) de plugin.
- **Parâmetros:** nenhum.
- **Retorno:** um `PluginFn` (importado de `@t8ngs/runner/types`).
- **Efeito:** quando o `PluginFn` retornado é executado pelo runner, ele registra
  a macro `expectTypeOf` no `TestContext`:

  ```ts
  TestContext.macro('expectTypeOf', expectType)
  ```

  Onde `expectType` é a função `expectTypeOf` importada da biblioteca
  [`expect-type`](https://www.npmjs.com/package/expect-type).

### Uso

```ts
import { expectTypeOf } from '@t8ngs/expect-type'
import { configure } from '@t8ngs/runner'

configure({
  plugins: [expectTypeOf()],
})
```

## Macro `expectTypeOf` no contexto de teste

Após o registro do plugin, cada teste recebe a propriedade `expectTypeOf` no seu
contexto:

```ts
test('título do teste', ({ expectTypeOf }) => {
  expectTypeOf({ foo: 'bar' }).toEqualTypeOf<{ foo: string }>()
})
```

A propriedade tem o mesmo tipo e API da função `expectTypeOf` da lib
`expect-type`. Alguns matchers comuns (fornecidos por `expect-type`):

| Matcher | Significado |
|---------|-------------|
| `.toEqualTypeOf<T>()` | O tipo é exatamente igual a `T` |
| `.toMatchTypeOf<T>()` | O tipo é atribuível a `T` |
| `.toBeString()` / `.toBeNumber()` / ... | Checagens de tipos primitivos |
| `.parameter(n)` | Inspeciona o enésimo parâmetro de uma função |
| `.returns` | Inspeciona o tipo de retorno de uma função |

> A lista completa de matchers pertence à biblioteca `expect-type`. Consulte a
> documentação dela para o conjunto completo e atualizado.

## Augmentação de tipos (TypeScript)

```ts
declare module '@t8ngs/runner/core' {
  interface TestContext {
    expectTypeOf: typeof expectType
  }
}
```

Essa declaração estende a interface `TestContext` do `@t8ngs/runner/core`,
adicionando a propriedade `expectTypeOf` com o tipo correto. É isso que habilita
o autocomplete e a checagem de tipos ao usar `({ expectTypeOf }) => ...`.

## Importante: sem runtime

As asserções **não são avaliadas em runtime**. Elas são verificadas pelo
compilador TypeScript. Para ver falhas, compile o projeto (`tsc` /
`npm run typecheck`).
