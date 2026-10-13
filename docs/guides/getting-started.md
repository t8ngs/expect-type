# Instalação e primeiros passos

Guia para começar a usar o `@t8ngs/expect-type` em um projeto que já utiliza o
`@t8ngs/runner`.

## Pré-requisitos

- **Node.js** `>= 18.16.0`.
- **TypeScript** no projeto (as asserções são verificadas na compilação).
- **`@t8ngs/runner`** instalado (é uma `peerDependency`, versão `^1.0.1`).

## 1. Instalação

```sh
npm i @t8ngs/expect-type
```

ou

```sh
yarn add @t8ngs/expect-type
```

## 2. Registrar o plugin

No arquivo de configuração do runner:

```ts
import { expectTypeOf } from '@t8ngs/expect-type'
import { configure } from '@t8ngs/runner'

configure({
  plugins: [expectTypeOf()],
})
```

## 3. Escrever asserções de tipo

Depois de registrar o plugin, `expectTypeOf` fica disponível no **contexto do
teste**:

```ts
test('objeto tem o shape esperado', ({ expectTypeOf }) => {
  expectTypeOf({ foo: 'bar' }).toEqualTypeOf<{ foo: string }>()
})
```

Outros exemplos:

```ts
test('tipos de função', ({ expectTypeOf }) => {
  const soma = (a: number, b: number): number => a + b

  expectTypeOf(soma).parameter(0).toBeNumber()
  expectTypeOf(soma).returns.toBeNumber()
})
```

## 4. Ver as falhas de asserção

As asserções **não falham em runtime**. Para verificá-las, compile o projeto:

```sh
npm run typecheck
# ou
npx tsc --noEmit
```

Se uma asserção de tipo for falsa, o **TypeScript** acusará o erro na compilação.

## Dúvidas comuns

- **"Rodei os testes e nada falhou, mas o tipo está errado."**
  Rode `npm run typecheck`. A verificação é feita pelo compilador, não em runtime.
- **"`expectTypeOf` não aparece no contexto."**
  Confirme que `expectTypeOf()` está no array `plugins` do `configure(...)` e que
  o `@t8ngs/runner` está instalado.

## Próximos passos

- [Referência da API](../references/api.md)
- [Visão geral da arquitetura](../architectures/overview.md)
