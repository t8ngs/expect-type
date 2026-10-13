# Guia de contribuição

Como contribuir com o `@t8ngs/expect-type`.

## Setup do ambiente

```sh
git clone https://github.com/t8ngs/expect-type.git
cd expect-type
npm install   # instala deps e, via "prepare", configura os git hooks (husky)
```

> `npm install` executa o script `prepare` (`husky`), que ativa os hooks de
> `pre-commit` e `commit-msg`.

## Fluxo de trabalho

1. Crie uma branch a partir de `main`.
2. Faça suas alterações no código.
3. **Atualize a documentação em `docs/`** correspondente à mudança (obrigatório —
   ver [`../../AGENT.md`](../../AGENT.md)).
4. Garanta qualidade localmente:
   ```sh
   npm run format      # formata
   npm run lint        # checa estilo/qualidade
   npm run typecheck   # checa tipos e asserções
   npm run build       # garante que o build passa
   ```
5. Faça o commit seguindo **Conventional Commits**.
6. Abra um Pull Request.

## Padrão de commits (Conventional Commits)

O projeto usa commitlint com `@commitlint/config-conventional`. Exemplos:

```
feat: adiciona matcher helper X
fix: corrige augmentação de tipo do TestContext
docs: atualiza referência da API
chore: atualiza dependências
```

Para commitar com assistente:

```sh
npm run commit   # abre o commitizen (cz)
```

O hook `commit-msg` valida automaticamente a mensagem. Commits fora do padrão
são rejeitados.

## O que a CI valida

- **`checks.yml`** valida as mensagens de commit (commitlint) em push/PR na `main`.

## Verificações obrigatórias antes do PR

| Verificação | Comando |
|-------------|---------|
| Formatação | `npm run format` |
| Lint | `npm run lint` |
| Tipos/asserções | `npm run typecheck` |
| Build | `npm run build` |

## Regra de documentação

> Nenhum PR que altere comportamento, API, scripts ou configuração deve ser
> aberto **sem** a atualização correspondente em `docs/`. Essa regra vale também
> para agentes de IA — ver [`../../AGENT.md`](../../AGENT.md) e
> [`../../CLAUDE.md`](../../CLAUDE.md).

## Sobre testes

Atualmente o script `test` é um placeholder (`echo "No tests"`). Se você adicionar
uma suíte de testes, atualize:

- O script `test` em `package.json`.
- [Referência de scripts](../references/scripts.md).
- O [roadmap](../planning/roadmap.md).
