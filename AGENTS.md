---
# AGENTS.md — developer-docs (Mintlify)

Instruções para agentes e autores que escrevem documentação neste repositório.

## Sobre o projeto

- Site Mintlify (`docs.json` + páginas MDX)
- Idioma: **pt-BR**
- Títulos em **sentence case**
- Frontmatter mínimo: `title`, `sidebarTitle`, `description`
- Componentes Mintlify quando úteis: `Steps`, `Note`, `Warning`, `Tip`, `Card`, `CardGroup`, `CodeGroup`

## Escopo Fiscal SDK (CT-e)

### API pública (obrigatório)

Documente como API padrão do consumidor:

1. `$cte->…->handle()` — client `Suot\Fiscal\Cte\Application\Cte`

Command/Query + Handler (`*Command::create()…->build()` / `*Query::create()…->build()` + `Handler::handle()`) só em:

- [Laravel Bridge](/fiscal/laravel-bridge) (seção avançada de Handlers)
- menções leves no glossário, se necessário

Não rotule seções como "Fluent", "Command equivalente", "Query equivalente", "dois entry points" ou "Entry point A/B". **Nunca** use a palavra "fluent" em copy publicada.

### Envelope obrigatório em todo snippet operacional

Todo exemplo que chama SEFAZ / Handler deve incluir:

1. `try/catch` de `Suot\Fiscal\Core\Exception\InfrastructureException` (com `toFailure()` quando útil)
2. Branches do Result (`isAuthorized` / `isRejected` / `isProcessing` / `isOperational` / `isValid` / `isFound`, conforme o tipo)
3. `failure()` e `retryPolicy()` / `RetryMode` quando a operação os expõe
4. Comentários do que o **host** deve fazer (persistir, corrigir, consultar antes de retry, filar delay, alertar, não reemitir)

### AfterCorrection na emissão

Nas páginas de capacidade CT-e: remontar `$cte->issue()…` com dados corrigidos e novo `forOperation` — não documentar `Command::revise()` nessas páginas.

Tip opcional sobre `revise()` apenas em `fiscal/rejeicoes.mdx` ou `fiscal/cte/resultados.mdx`, com link ao Laravel Bridge se precisar.

### Contingência

`issueInContingency()->issue($command)` exige `IssueCteCommand`. Mostre `$cte->issue()…->build()` e depois a cadeia de contingência, com envelope completo de Result.

### Nunca documentar como API do consumidor

- `CTeBuilder`, `EmitirCTe*`, `AutorizarNFe*`, `AutorizarCte*`
- `Operation/*` como entry point (`*Input`, `*::execute()`, `*Service`)
- `*InputMapper`, `IssueCteCommandBuilder`, `IssueCteCommandParts`
- `CteClient` como porta principal

### Namespaces

- Application: `Suot\Fiscal\Cte\Application\…`
- Data / VO CT-e: `Suot\Fiscal\Cte\Data\…`, `Suot\Fiscal\Cte\ValueObject\…`
- Core VO: `Suot\Fiscal\Core\ValueObject\…`
- Results: `Suot\Fiscal\Cte\Operation\…\*Result` (retorno público; pasta `Operation` de **serviços** continua interna)

### Pacotes Composer

Use `usesuot/fiscal-core`, `usesuot/fiscal-sefaz`, `usesuot/fiscal-cte`, `usesuot/fiscal-laravel` — não `suot/fiscal-*` inventado.

### Licença e prontidão

- Ler docs **não** exige licença; `composer require` no registry **exige**
- Não declare production-ready sem caveat (schemas, fixtures, ciclo de vida, homologação)
- Vencimento de update ≠ kill switch no runtime instalado

### Página de capacidade (padrão)

1. O quê / quando
2. Exemplo `$cte->…->handle()` **inline no MDX** (com envelope completo)
3. Como ler o Result / o que o host faz
4. Link para outras páginas da docs (nunca para arquivos do repositório)

### Fonte de verdade do código

Autores **podem** ler `../fiscal-sdk-php` (código-fonte em `packages/`) para garantir precisão de API. **Nunca** cite `examples/`, `bin/`, `bin/dx/` ou caminhos de monorepo em páginas publicadas. Todo snippet de uso vive **inline no MDX** — o consumidor recebe só pacotes Composer (`usesuot/fiscal-*`).

## Estilo

- Ativo, 2ª pessoa, frases curtas
- Precisão de código > marketing
- Signal (`#FB0927`) é marca, não cor de erro — na docs, use `Warning`/`Tip` sem metáfora de cor
- Sem emoji

## Navegação

Ao criar páginas novas, atualize `docs.json`. Remova stubs órfãos (ex.: `fiscal/cte.mdx` se existir `fiscal/cte/index.mdx`).
