# Simulated Portfolio

Documentation for a simulated paper-trading setup. This repository contains
documentation only — it contains no trading code, no live trading, and no
connection to a broker.

**Simulated only. No real money. Not financial advice.**

## Overview

Four simulated portfolios, each starting at $30,000 USD (a $10,000 start on
30 October 2026, plus a $20,000 cash infusion to each on the same day):

| Portfolio | What it trades | Engine |
| --- | --- | --- |
| Main | Large, liquid US stocks | Automatic rules, checked every 5 minutes |
| Obscure | Smaller, more volatile US stocks | Automatic rules, checked every 5 minutes |
| Crypto | Altcoins (24/7) | Automatic rules, checked every 5 minutes |
| Crypto AI Agent | Altcoins (24/7) | Agent decisions, checked every 15 minutes |

Total simulated capital across the four portfolios: $120,000 USD.

Bitcoin and Ethereum are deliberately excluded from every crypto watchlist
and strategy.

## Watchlists

### Main (10 stocks)

NVDA, AAPL, MSFT, AMZN, META, GOOGL, AMD, TSLA, AVGO, PLTR

### Obscure (10 stocks)

SOUN, BBAI, RGTI, QBTS, LUNR, SERV, RCAT, KULR, ACHR, SMR

### Crypto and Crypto AI Agent (20 coins, shared watchlist)

SOL, XRP, DOGE, ADA, AVAX, SUI, XLM, HBAR, DOT, NEAR, TRX, APT, ARB, OP,
INJ, SEI, TIA, FIL, ICP, ALGO

## Trading rules (automatic portfolios)

- **Momentum entry:** buy a name meeting the profile's momentum trigger and
  near the top of its intraday range. For crypto only, the price only needs
  to be in the top half of its intraday range (the percentage trigger is
  unchanged).
- **Dip entry:** a name down 2.0% or more on the day and at least 2.5%
  below its intraday high is bought as a dip entry. Momentum entries are
  considered first each cycle; dips use the capacity left over.
- **Position sizing:** each new buy gets an equal share of available cash
  across the position slots still free, capped at the profile's
  position-size percentage.
- **Exits:** per-profile take-profit / stop-loss. Stock positions are
  intended to be flattened before the 4:00 PM ET close.
- **Unlimited entries:** switched on for Main, Obscure and Crypto — new
  positions are limited only by max open positions, one position per
  ticker, spread sizing and available cash. Picking a named risk profile
  turns unlimited entries off, since profiles carry their own finite caps.

## Risk profiles

Entries per day are shown as stocks / crypto (crypto caps are double).

| Profile | Take-profit / Stop-loss | Size | Max positions | Trigger | Entries/day |
| --- | --- | --- | --- | --- | --- |
| Conservative — Capital Keeper | +1.5% / −0.8% | 10% | 2 | +1.2% | 3 / 6 |
| Balanced — Steady Trader | +2.0% / −1.2% | 20% | 3 | +0.8% | 6 / 12 |
| Aggressive — Growth Seeker | +3.5% / −2.0% | 30% | 4 | +0.6% | 8 / 16 |
| Speculative — Moonshot | +6% / −3% | 40% | 5 | +0.4% | 10 / 20 |
| Extreme — Full Send | +10% / −5% | 50% | 10 | +0.2% | 12 / 24 |

Current production settings (shown as Custom in the site, as they have
been edited from the named profiles):

- **Main:** take-profit +3.5%, stop-loss −2%, size 20%, max 10 positions,
  trigger +0.6%, unlimited entries on.
- **Obscure:** take-profit +6%, stop-loss −3%, size 20%, max 5 positions,
  trigger +0.4%, unlimited entries on.
- **Crypto:** Extreme profile, unlimited entries on.

## Crypto AI Agent (agent-traded)

The Crypto AI Agent portfolio is traded by agent judgement, not by the
automatic rule engine. The automatic cycle only refreshes its quotes, news
and equity curve.

- Checks every 15 minutes.
- Unlimited open positions (in practice, at most one position per coin on
  its watchlist). No fixed per-buy percentage cap — a single buy is limited
  only by available cash.
- **Sizing:** dynamic, by confidence in the setup — no strict percentage.
- **Strategy — dip-first + momentum hybrid:**
  1. Dips first — coins down on the day, especially −2% or more, only
     when sentiment supports a rebound. Never buy a falling knife on
     purely negative news.
  2. Momentum second — coins up with supportive news and room to run.
     Do not chase a coin at or near its day high with no fresh catalyst.
  3. Exits — do not sell at the first pullback; cut only on a clear
     reversal, a breakdown below support, or a larger loss.
- **Aggressive mode (approved 31 October 2026):** up to 3 new positions
  per run, aim to deploy most idle cash, borderline/moderate setups
  qualify, exits looser.
- **Rotation (approved 1 November 2026, superseding a 31 October decline):**
  when cash is $0 or too low for a qualifying new setup, the agent may
  sell the weakest open position to free cash for a clearly stronger
  qualifying dip or momentum setup. A rotation is a sell plus a buy in the
  same run, counts toward the 3-new-position limit, and is logged with
  both reasons. No rotation on small noise or a marginal upgrade, and no
  repeated rotation of the same coin within a day.

## Notes

- The live portfolio site itself is a hosted Muse web artifact
  (slug `simulated-portfolio`), not code in this repository.
- Position, cash and equity figures change with every trading cycle;
  this README records the strategies and settings, not a live balance.
- Strategy, risk-profile and starting-amount changes are only made with
  the owner's explicit say-so.
