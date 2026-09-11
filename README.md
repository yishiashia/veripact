# VeriPact

**English** | [繁體中文](README.zh-TW.md)

**Verifiable delivery for coding agents.**

Coding agents now produce changes faster than any human can read them. The bottleneck is no longer writing code — it is *trusting* what was written. VeriPact is a local-first, Git-backed control layer designed to make an agent's delivery **traceable and verifiable**: what was agreed, what changed, what evidence actually ran, who reviewed it, and who holds the authority to let it proceed.

It helps humans supervise *outcomes* without pretending that passing checks prove correctness, or that a review automatically grants merge or deployment authority.

> **Status: early / concept.** This repository currently documents the design and trust model. The implementation is being developed privately and is not yet published here. Watch or star to follow the release.

This is an independent community project. It is not affiliated with GitHub, OpenAI, Anthropic, or any coding-agent vendor.

## The problem

When an agent says "done," you inherit four questions that a green check mark cannot answer:

1. Did it build what we actually agreed to?
2. Did the checks it cites really run against *this* code?
3. Did anyone independently confirm the claim, or did we just believe it?
4. Who decided this was allowed to ship — and did a `git push` quietly become that decision?

VeriPact separates these into four verifiable concerns, each kept inspectable in Git rather than trapped in a chat log.

## The trust model

VeriPact is built on four pillars. Each one is developed in depth in a companion essay (in Traditional Chinese).

### Contract — *what was agreed*

Specifications, plans, non-goals, architecture decisions, acceptance criteria, and other governed requirements define what the agent is expected to deliver. The spec is not a prompt that evaporates — it is a durable contract with a version baseline that later work can be checked against.

→ [Spec as Contract](https://yishiashia.github.io/posts/spec-as-contract/)

### Evidence — *what actually ran*

The model binds governed checks to the exact source revision and the execution conditions or policy that produced them. Evidence is intended to be reproducible and tamper-evident: a result is not enough unless it can be shown to correspond to *this* code under *these* recorded conditions.

→ [Reproducible Verification and Attestation](https://yishiashia.github.io/posts/reproducible-verification-and-attestation/)

### Attestation — *who independently confirmed it*

Independent review records cited findings and requirement coverage instead of accepting an agent's own completion claim. Under the selected governance policy, approval must address the governed requirements applicable to that review — coverage is checked, not assumed. Independence is about role and authority separation, not merely using another model or agent name.

→ [Reproducible Verification and Attestation](https://yishiashia.github.io/posts/reproducible-verification-and-attestation/)

### Authority — *who is allowed to proceed*

Commit, push, merge, publish, and deploy are separate authorization decisions. A commit grants none of the later actions; a push grants none of them either. Depending on risk and policy, a decision may be made by an authorized human or exercised automatically under a pre-approved policy. Automation may exercise delegated authority — it does not create it.

→ [Authority Boundary: Human on the Loop](https://yishiashia.github.io/posts/authority-boundary-human-on-the-loop/)

## Harness engineering, not prompt engineering

The reliability of agent work does not come from a cleverer prompt — it comes from the **harness** the agent runs inside: the gates, evidence bindings, and authority boundaries that constrain what an agent can silently do. VeriPact treats that harness as an engineering and governance problem in its own right.

→ [Harness Engineering and Governance](https://yishiashia.github.io/posts/harness-engineering-governance/)

The current VeriPact design expresses risk-based governance through three workflow modes:

| Mode | Intended use |
| --- | --- |
| **Fast** | Small, low-risk, bounded change with an automated check |
| **Standard** | A normal feature, or an incomplete brief |
| **Strict** | Security, payment, privacy, permissions, schema, migration, public API, and other high-risk work — full gates with explicit human decisions where required |

Workflows can *upgrade* but never silently *downgrade*. The verifiable substrate stays the same at every level.

These modes are a VeriPact product design choice, not additional pillars in the trust model.

## What it is designed to be (and not be)

- It is designed to **coordinate** existing coding agents (via MCP) while keeping governed state in Git.
- It does **not** provide or invoke an LLM by itself.
- It does **not** infer merge or deployment authority merely because checks or reviews passed.
- It does **not** claim evidence proves perfect correctness — only that claims can be traced back to explicit contracts, evidence, attestations, and authority decisions.

## Roadmap

- [x] Trust model and design
- [ ] Public documentation of the workflow and gates
- [ ] Reference implementation (MCP server + CLI)
- [ ] Examples and integration guides

## Author

Written by [yishiashia](https://yishiashia.github.io). The four essays above develop the ideas behind VeriPact in depth.

## License

This repository currently contains **concept documentation, not public source code**.

- The **documentation and written content** are licensed under the [Creative Commons Attribution 4.0 International License (CC BY 4.0)](LICENSE-DOCS).
- The future **reference implementation source code** is intended to be released under the [MIT License](LICENSE-CODE).
- See [LICENSE](LICENSE) for the repository-wide licensing scope.
