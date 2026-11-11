# Trading

The common trading vocabulary -- the brokerage accounts that trade, the orders they place, the fills that execute them, the positions they hold, the portfolios and watchlists that organise them -- distilled from the APIs that already exist. Market prices belong to the market-data context. Part of [Schema Commons](https://github.com/api-commons/schema-commons),
published by the [API Commons](https://apicommons.org).

Built from the providers tagged Trading, Brokerage, Investing, Crypto Exchange, Online Brokerage, Algorithmic Trading, Wealth Management, Investment Platform in [APIs.io](https://apis.io) that
publish their own OpenAPI. Only harvested contracts count -- nothing API Evangelist
generated. Every number below is a count of providers.

## What the market already agrees on

<!-- commons:summary -->
| Concept | Expose it | Readable schema | Core properties (share) | Collection named |
| --- | ---: | ---: | --- | --- |
| trade_order | 38 | 36 | id 94%, type 94%, quantity 92%, price 89%, side 86%, symbol 78%, time_in_force 75%, created_at 72%, stop_price 58% | orders 36, order 12, open_orders 2, openorders 1 |
| fill | 18 | 16 | order_id 94%, price 81%, id 75%, quantity 75%, symbol 75%, side 62%, executed_at 56%, fee 56% | fills 8, executions 4, fill 2, mytrades 2 |
| position | 51 | 48 | quantity 67%, symbol 58% | positions 41, holdings 11, position 6, holding 2 |
| brokerage_account | 39 | 38 | type 55% | accounts 23, account 16, subaccounts 5, sub_accounts 1 |
| portfolio | 7 | 7 | name 71%, account_id 57%, id 57% | portfolios 6, portfolio 3 |
| watchlist | 8 | 6 | id 83%, name 83%, created_at 50%, total_items 50% | watchlists 8 |

Cohort: 174 providers with harvested OpenAPI, 51 crosswalks. Of the providers exposing a trade_order: 92% create, 92% list, 71% fetch, 34% update, 55% delete. Generated 2026-11-11 by build.py.
<!-- /commons:summary -->

Shares are of the providers with a readable resource schema for that concept. The
extended tier and the long tail of every concept are in [profile.yml](profile.yml).

## What the measurement shows

- A trade order agrees on more than any order in the commons: side, symbol, quantity, price, type, time in force and stop price are all core. Brokers and crypto exchanges inherited the order ticket from FIX, and it shows.
- A fill agrees just as completely -- order, price, quantity, side, symbol, execution time and fee.
- A position agrees on symbol and quantity; cost basis, market value and P&L are each named differently by brokers, exchanges and wealth platforms.
- A brokerage account agrees only on its type: buying power, cash and equity are reported by every broker under its own names.

## Artifacts

- [openapi.yml](openapi.yml) -- OpenAPI 3.1 of the shared operations. No one serves it; it
  is the shape to map to. Lints clean against the
  [naming ruleset](https://github.com/api-commons/schema-commons/blob/main/ruleset/schema-commons-naming.yaml).
- [schema/](schema) -- JSON Schema 2020-12 per concept; each property carries
  `x-commons-tier`, `x-commons-share`, `x-commons-providers` and the spellings seen.
- [crosswalk/](crosswalk) -- one per provider, common name -> their name, with the harvested
  spec and operations each mapping was read from.
- [adopters.yml](adopters.yml) -- who exposes which concept, linked to their APIs.io page.
- [concepts.yml](concepts.yml) -- the only hand-written input: concepts, path segments and
  their qualifiers, aliases, reviewed exclusions, merged duplicates, and the decisions below.
- [census/](census) -- the cohort and the raw census the profile is built from.

## Decisions

These are calls, not measurements, recorded so they can be revisited as the method matures.

- Orders are `/trade-orders` and accounts `/brokerage-accounts`: commerce owns `/orders` and banking `/accounts`. Trading orders were excluded from commerce for exactly this reason.
- Fills cover executions and an account's own trades (`myTrades`); public market trades are the market-data context's.
- Holdings are positions.
- Orders, fills, accounts, portfolios and watchlists `require: [position]`: a provider that trades holds positions, and the requirement keeps commerce's orders out.
- Duplicates merged: Arch, Bamboo.

## Rebuild

```bash
../schema-commons/scripts/run.sh .
```

## License

Artifacts CC BY-NC-SA 4.0 ([LICENSE](LICENSE)); code Apache 2.0 ([LICENSE-CODE](LICENSE-CODE)).
