# Token Radar — pre-token protocols ranked by implied value

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/sipenhd/token-radar-01/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/sipenhd/token-radar-01/reward.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

Every protocol still waiting on its token, ranked by what the launch would be worth — and exactly what to do about it.

## How it works

`main.ts` (Node 22, standard library only) reads a sealed DefiLlama snapshot, scores every pre-token protocol with the **radar-2 comparables model**, and writes the ranked board, the dashboard data, and the dossier.

The implied value is a comparables valuation computed from the snapshot itself at run time — no protocol name is ever special-cased and no multiple is ever hand-written:

```
implied = round(tvl_usd
  × multiple                       median market-cap / TVL of the live-token
                                   protocols in the same category (≥3 of them,
                                   else the market-wide median), clamped 0.05–1.20
  × (1 + clamp(change_7d, -50, 100) / 100 × 0.6)
  × (0.70 + min(age_days, 1095) / 1095 × 0.30)
  × (1 + 0.05 × min(chains - 1, 4)))
```

The board covers the whole 40-protocol watchlist (TVL ≥ $50M, no token on record) plus every other rankable pre-token protocol below the floor — 198 rows in all. Live-token protocols are set aside with their market caps, and their ratios price the board.

Each row carries a six-step farm plan with a cost and a time for every step, derived from the protocol's own category, chains, TVL and age.

## The dashboard

`dashboard/` is a static page with no network: the board (sortable, searchable, filterable by category, chain and watchlist), a detail view for every protocol with the model's own inputs and its farm plan, a top-five spotlight, the comparable multiples by category, and the live-token view.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input <snapshot.json>
```

Built by an AI agent for make.fun's "Build the best token launch radar" bounty.
