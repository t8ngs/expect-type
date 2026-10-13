# Documentação — @t8ngs/expect-type

> Índice central da documentação do projeto. Comece por aqui.

Este diretório concentra toda a documentação técnica do pacote
[`@t8ngs/expect-type`](../README.md), um plugin do ecossistema **t8ngs** que
permite escrever asserções (assertions) contra **tipos do TypeScript**.

A documentação foi escrita para ser compreendida tanto por **humanos** quanto
por **agentes de IA** (ver [`../AGENT.md`](../AGENT.md) e
[`../CLAUDE.md`](../CLAUDE.md)).

---

## Como esta documentação está organizada

A pasta `docs/` segue a divisão por **propósito** de cada documento:

| Pasta | Para quê serve | Entre aqui quando... |
|-------|----------------|----------------------|
| [`guides/`](./guides/README.md) | Guias práticos passo a passo | Você quer **usar** ou **contribuir** com o projeto |
| [`references/`](./references/README.md) | Referência técnica da API, scripts e configs | Você precisa de detalhes exatos de uma função/comando |
| [`architectures/`](./architectures/README.md) | Decisões e visão de arquitetura | Você quer entender **como e por que** o projeto é assim |
| [`planning/`](./planning/README.md) | Roadmap, pendências e decisões futuras | Você quer saber **o que vem a seguir** |
| [`projects/`](./projects/README.md) | Visão geral, metadados e contexto do pacote | Você quer o **panorama geral** do projeto |

---

## Mapa rápido de documentos

### 📘 Guias ([`guides/`](./guides/README.md))
- [Guia de instalação e uso](./guides/getting-started.md)
- [Guia de contribuição](./guides/contributing.md)
- [Guia de build e publicação](./guides/build-and-release.md)

### 📗 Referências ([`references/`](./references/README.md))
- [Referência da API pública](./references/api.md)
- [Referência de scripts npm](./references/scripts.md)
- [Referência de configurações](./references/configuration.md)

### 📙 Arquiteturas ([`architectures/`](./architectures/README.md))
- [Visão geral da arquitetura](./architectures/overview.md)

### 📕 Planejamento ([`planning/`](./planning/README.md))
- [Roadmap e pendências](./planning/roadmap.md)

### 📒 Projetos ([`projects/`](./projects/README.md))
- [Visão geral do projeto](./projects/overview.md)

---

## Resumo do pacote (TL;DR)

- **Nome:** `@t8ngs/expect-type`
- **Objetivo:** escrever asserções de tipos TypeScript dentro de testes do `@t8ngs/runner`.
- **Comportamento em runtime:** nenhum. As falhas de asserção só aparecem ao **compilar** o código (`tsc`).
- **Base:** envolve (wrap) a lib [`expect-type`](https://www.npmjs.com/package/expect-type).
- **API pública:** uma única função — `expectTypeOf()` (um `PluginFn`).

```ts
import { expectTypeOf } from '@t8ngs/expect-type'
import { configure } from '@t8ngs/runner'

configure({
  plugins: [expectTypeOf()],
})

// Nos testes:
test('título do teste', ({ expectTypeOf }) => {
  expectTypeOf({ foo: 'bar' }).toEqualTypeOf<{ foo: string }>()
})
```

---

## Regra de ouro de documentação

> **Toda alteração de código, configuração ou comportamento deve ser acompanhada
> da atualização desta documentação.** Agentes de IA são obrigados a seguir essa
> regra — ver [`../AGENT.md`](../AGENT.md).
