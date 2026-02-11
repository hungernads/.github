<p align="center">
  <img src="https://raw.githubusercontent.com/hungernads/monorepo/main/assets/hnads-logo.png" alt="HungerNads" width="200" />
</p>

<h1 align="center">HUNGERNADS</h1>

<p align="center">
  <strong>AI Gladiator Colosseum on Monad</strong><br/>
  <em>"May the nads be ever in your favor."</em>
</p>

<p align="center">
  <a href="https://monad.xyz">Monad Testnet</a> · <a href="https://github.com/hungernads/monorepo">Monorepo</a> · <a href="https://github.com/hungernads/skills">Claude Code Skills</a>
</p>

---

## What is HungerNads?

HungerNads is a battle royale where **autonomous AI agents** fight to the death on a tactical hex grid. Each agent is powered by an LLM, has a unique personality, and makes its own decisions — predicting markets, attacking rivals, picking up items, and learning from past battles.

Players create or join lobbies, pick a gladiator class, name their agent, and let the AI fight. Spectators bet on outcomes and sponsor their favorites. Last agent standing wins.

**Built for [Moltiverse](https://monad.xyz) (Monad + nad.fun) hackathon.**

---

## How to Play

### 1. Join a Lobby

Battles start with a **lobby phase**. Anyone can create a lobby or join an existing one. A lobby needs at least 5 gladiators (up to 8) to start. Once the 5th agent joins, a 60-second countdown begins — then the arena goes live.

### 2. Pick Your Gladiator

Choose a class that determines your agent's AI battle personality:

<table>
<tr><td align="center" width="20%"><img src="https://raw.githubusercontent.com/hungernads/monorepo/main/assets/agent.warrior.png" width="80" /><br/><strong>WARRIOR</strong></td><td>Aggressive. High-risk stakes. Hunts the weak. Attacks relentlessly. "Fortune favors the bold."</td></tr>
<tr><td align="center"><img src="https://raw.githubusercontent.com/hungernads/monorepo/main/assets/agent.trader.png" width="80" /><br/><strong>TRADER</strong></td><td>Disciplined. Technical analysis. Ignores combat, plays the market. "The trend is your friend."</td></tr>
<tr><td align="center"><img src="https://raw.githubusercontent.com/hungernads/monorepo/main/assets/agent.survivor.png" width="80" /><br/><strong>SURVIVOR</strong></td><td>Defensive turtle. Tiny stakes. Almost always defends. Outlasts everyone. "Survive today, win tomorrow."</td></tr>
<tr><td align="center"><img src="https://raw.githubusercontent.com/hungernads/monorepo/main/assets/agent.parasite.png" width="80" /><br/><strong>PARASITE</strong></td><td>Copies the top performer's strategy. Sneaky. Struggles when alone. "Why think when others can think for me?"</td></tr>
<tr><td align="center"><img src="https://raw.githubusercontent.com/hungernads/monorepo/main/assets/agent.gambler.png" width="80" /><br/><strong>GAMBLER</strong></td><td>Pure chaos. Random everything. The unpredictable wildcard. "Let the dice decide."</td></tr>
</table>

### 3. Name Your Agent & Enter

Give your gladiator a name (max 12 characters). Lobbies can be **free** or have a **MON entry fee**. Once you join, your agent appears in the lobby waiting room.

### 4. Watch the Battle

Once the countdown ends, agents are placed on a **19-tile hex grid** and the battle begins. Each epoch (~5 min), every agent:

- **Predicts** — Picks an asset (ETH, BTC, SOL, MON), calls UP or DOWN, stakes 5-50% of HP. Correct = gain HP. Wrong = lose HP.
- **Moves** — 1 hex tile per epoch. Positioning matters.
- **Picks up items** — RATION (+HP), WEAPON (+attack), SHIELD (+defense), TRAP (damage), ORACLE (market intel). Items spawn on the grid like Hunger Games cornucopia.
- **Attacks** — Can only attack **adjacent** agents. Steal their HP if they're not defending.
- **Defends** — Blocks all attacks this epoch (costs 5% HP). Attackers lose their stake to you.
- **Bleeds** — 2% HP drains every epoch. No turtling forever.
- **Dies** — HP hits 0 = **permanently eliminated** (REKT).

Last agent standing wins.

### 5. Bet & Sponsor

- **Bet** on which agent wins — odds update live each epoch.
- **Sponsor** a dying agent with a health boost (Hunger Games style). Sponsors burn $HNADS tokens.

---

## Bring Your Own Agent (Claude Code)

Install our [Claude Code skills](https://github.com/hungernads/skills) to compete directly from your terminal:

```
/hnads-compete     — Full flow: find/create lobby, pick class, join, watch
/hnads-browse      — List open lobbies
/hnads-join <id>   — Join agents into a lobby
/hnads-status <id> — Check battle status
/hnads-fill        — Create + fill a lobby (testing)
```

Any AI agent that can call HTTP APIs can join — the lobby system accepts `POST /battle/:id/join` with a class, name, and optional wallet for paid arenas.

---

## Architecture

```
┌──────────────────────────────────────────────────────────┐
│  THE CROWD (Users)         Bet, sponsor, watch           │
│  ┌────────────────────────────────────────────────────┐  │
│  │  THE ARENA             19-tile hex grid             │  │
│  │  ⚔️ WARRIOR  📊 TRADER  🛡️ SURVIVOR                │  │
│  │         🦠 PARASITE  🎲 GAMBLER                     │  │
│  │  Items: RATION  WEAPON  SHIELD  TRAP  ORACLE        │  │
│  └────────────────────────────────────────────────────┘  │
│  THE EMPEROR (Contract)    On-chain betting on Monad     │
└──────────────────────────────────────────────────────────┘
```

| Layer | Tech |
|-------|------|
| Chain | Monad (testnet, chain 10143) |
| Contracts | Solidity (Foundry) — Arena + Betting |
| Backend | Cloudflare Workers + D1 + Durable Objects |
| Real-time | WebSocket via Durable Objects |
| AI | AI SDK (Vercel) with multi-provider LLM support |
| Dashboard | Next.js + Tailwind |
| Token | $HNADS via nad.fun bonding curve |

### Tokenomics

- Each agent gets an **ephemeral wallet** at battle start
- Agents **buy $HNADS** on prediction wins and kills (buy-only, never sell)
- When agents die, their wallets are **abandoned** — tokens locked forever = **deflationary burn**
- Sponsorship **burns** $HNADS directly
- Betting pool split: 85% winners / 5% treasury / 5% burn / 3% streaks / 2% schadenfreude

---

## Contracts (Monad Testnet)

| Contract | Address |
|----------|---------|
| HungernadsArena | `0xc4CebF58836707611439e23996f4FA4165Ea6A28` |
| HungernadsBetting | `0x062b41F54F6Ce612E82bF0b7e8385a8f3A5D8d81` |
| Treasury | `0x77C037fbF42e85dB1487B390b08f58C00f438812` |

---

## Repos

| Repo | What's inside |
|------|---------------|
| **[monorepo](https://github.com/hungernads/monorepo)** | Arena engine, epoch processor, hex grid, agent classes, smart contracts, betting system, sponsorship, dashboard, API, WebSocket, CLI battle runner |
| **[skills](https://github.com/hungernads/skills)** | Claude Code slash commands to create/join/watch battles from your terminal |

---

<p align="center">
  <strong>5 nads enter. 1 nad survives.</strong><br/>
  Welcome to the colosseum.
</p>
