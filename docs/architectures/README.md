# Arquiteturas

> Decisões e visão de arquitetura do `@t8ngs/expect-type`.

Esta pasta documenta **como** o projeto é estruturado e **por que** cada decisão
foi tomada.

## Documentos

- [Visão geral da arquitetura](./overview.md) — fluxo do plugin, integração com
  o `@t8ngs/runner`, estratégia de build e por que não há comportamento em runtime.

## Quando atualizar

Atualize os documentos desta pasta sempre que:

- A forma como o plugin se integra ao runner mudar.
- A estratégia de build (tsdown / `tsc`) for alterada.
- Novos módulos, camadas ou pontos de extensão forem adicionados.
