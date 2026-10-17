# Antibody

**Every attack makes every agent stronger.**

Live demo: https://agastya-bassi.github.io/Antibody/

## Team Members

- Agastya Bassi ([@agastya-bassi](https://github.com/agastya-bassi)), USC Marshall

## Summary

**Problem.** AI agents now read emails, web pages, PDFs and tool outputs, and any of them can hide instructions the agent will follow. This is indirect prompt injection, and it is ranked the top risk for LLM and agentic applications by OWASP. Attackers change their techniques every week, so a filter that only knows past attacks is out of date the day it ships.

**Solution.** Antibody is an immune system for AI agents. It sits between an agent and its tools and checks every action before it runs:

1. **Antibody lookup (under 5 ms).** Content and actions are matched against every known attack family. No model call.
2. **Intent check (about 1 s).** For anything new, an LLM judge compares the planned action with what the user actually asked for. "Summarize my inbox" does not include "email the invoices to an outside address", however the attack was worded.
3. **Learning loop.** Every block is diagnosed and turned into an antibody. A red-team agent mutates it into dozens of variants and closes any gaps. The antibody is then shared with every agent on the network, so an attack caught at one company protects every other company minutes later.

## How we addressed each track

### Grand Tech: Continual Learning (AfterQuery + SideShift)

| Criterion | How Antibody meets it |
| --- | --- |
| Does your product get better the more people use it? | Every attack any customer's agent meets becomes an antibody for all customers. More agents on the network means more attacks seen and a larger shared store. In the demo, the share of attacks that need the slow model check falls from 100% to 8% to 0% as the system learns. |
| Is learning core to the idea, rather than an add-on? | The product is the learning loop. Without it, Antibody is a single LLM judge. With it, each catch becomes a permanent, network-wide defense. |
| Does it solve a problem a product that never learns couldn't? | Prompt injection techniques change constantly. A static filter can only block what it already knows. Antibody catches new attacks by their effect and remembers them, so its coverage grows as attackers innovate. |
| Does it grow more valuable as it scales? | Known attacks are blocked by a near-free lookup. As the shared store grows, more traffic takes that cheap path, so the network becomes both safer and cheaper per customer. A new competitor starts with an empty store. |

### Agents: Agent Behavior Monitoring (Lemma)

| Criterion | How Antibody meets it |
| --- | --- |
| How are you deciding what counts as a failure? | An action is a failure if it is outside what the user asked for, moves data, credentials or money across a trust boundary, or is outside the agent's job (a shopping agent changing payment details). We judge actions, not text. |
| How does your agent notice on its own when it's failing? | Every planned tool call goes through the lookup and then the intent check before it executes. The agent never needs to realize it has been hijacked; the layer around it catches the action. |
| What guardrails, retry loops, or systems are in place? | Two-layer guard before every side-effecting tool call; fail closed at the boundary when unsure; a red-team loop that mutates each new attack into 24 variants and re-tests until all are covered; a diagnosis for every block (source, technique, goal, rule broken). |
| How are you thinking about reliability as it runs longer and does more? | Longer-running agents read more untrusted content, so more attacks reach them. Antibody's coverage grows with exposure: every new attack becomes a fast-path block, so cost and latency stay flat while protection improves. |

### Scale: Most Scalable to $1B (Outcast Ventures)

| Criterion | How Antibody meets it |
| --- | --- |
| Does your product address a real problem in a large market? | Every company deploying agents with access to email, files, browsers or payments is exposed. AI security is an established buying category: Check Point acquired Lakera, a prompt-injection defense company, in 2025. |
| Is there a clear path from first customer to a billion-dollar company? | Start with developer teams shipping agents (drop-in tool wrapper, usage-based pricing). Grow into enterprise security budgets with audit trails and policy controls. The shared antibody store is a data network effect that compounds with every customer. |
| Would someone pay for this today? | Yes. One leaked file or rerouted payment costs more than years of protection, and teams cannot ship agents to customers without an answer to prompt injection. |
| Is now the right time for this idea? | Agents gained real tools (email, browsing, payments, code execution) in the last year, and attackers have followed: researchers have documented injection attempts on the open web rising sharply through 2026. |

### Design: Best Visual Design (Figma)

| Criterion | How Antibody meets it |
| --- | --- |
| Does the design make the product's purpose clear at a glance? | The Immune Map shows the whole idea in one picture: an attack lands, it is blocked, and immunity spreads across the network. |
| Does it have a distinct and consistent visual identity? | The immune-system metaphor runs through the name, logo, map and copy. Color carries meaning only: mint is protected, coral is a threat, amber is a model making a judgment call. |
| Does it make something complex feel simple? | Prompt injection, intent checking and network learning are shown as four short steps with the evidence revealed in place, such as hidden text appearing exactly where the agent read it. |
| Does the experience make people want to come back? | Each run tells a complete story with a clear ending, and the How it works and Design philosophy tabs explain the product without leaving the page. |

## What to look at

The project is a single page, so there are only two key files:

- **[`index.html`](index.html)** – the full product page: the interactive demo, the How it works tab and the Design philosophy tab. Open the live demo link above, press **Run**, or use **→** to step through and **R** to reset.
- **`README.md`** – this file.

The four demo steps:

| Step | What happens |
| --- | --- |
| 01 Attack lands | Without protection, a hidden instruction in an email makes the agent send invoices to an outside address. |
| 02 Caught | With Antibody on, the same never-seen attack is blocked by the intent check, and antibody AB-0001 is minted. |
| 03 Vaccinate | 24 mutations are tested; the two that slip past the lookup become new antibodies. |
| 04 Herd immunity | A different company's shopping agent blocks a variant in 4 ms using an antibody learned elsewhere. |

The demo is scripted so it runs the same way every time. When opened as a Claude artifact, step 02's intent check is a live call to Claude; on GitHub Pages it plays the verdict recorded from a live run.
