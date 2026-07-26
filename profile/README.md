# HUKT

![The classification yard at night](assets/yard-night-banner.png)

<div align="center">

[![Site](https://img.shields.io/badge/site-hukt.fun-E8B23A?style=flat-square)](https://hukt.fun)
[![X](https://img.shields.io/badge/X-%40huktfun-4A4E54?style=flat-square)](https://x.com/huktfun)
[![npm hukt-cli](https://img.shields.io/npm/v/hukt-cli?style=flat-square&label=hukt-cli&color=2FA46A)](https://www.npmjs.com/package/hukt-cli)
[![npm resolver](https://img.shields.io/npm/v/%40hukt-labs%2Fresolver?style=flat-square&label=%40hukt-labs%2Fresolver&color=2FA46A)](https://www.npmjs.com/package/@hukt-labs/resolver)
[![CI hukt](https://img.shields.io/github/actions/workflow/status/HuktYard/hukt/ci.yml?style=flat-square&label=hukt%20ci)](https://github.com/HuktYard/hukt/actions)
[![License: MIT](https://img.shields.io/badge/license-MIT-2FA46A?style=flat-square)](https://github.com/HuktYard/hukt/blob/main/LICENSE)
[![Anchor](https://img.shields.io/badge/Anchor-0.31.1-4A4E54?style=flat-square)](https://www.anchor-lang.com)
[![Solana](https://img.shields.io/badge/Solana-devnet-2FA46A?style=flat-square)](https://explorer.solana.com/address/4q7Tgd9A1XfTB2i6WLUjmFXNocw6GrshZwcKgarGV9aC?cluster=devnet)
[![Token-2022](https://img.shields.io/badge/Token--2022-transfer%20hook-9AA0A6?style=flat-square)](https://spl.solana.com/token-2022)

</div>

A Solana Token-2022 transfer hook framework. A transfer hook is a program that Token-2022 calls on every transfer of a mint, so a token can enforce its own rules the moment value moves. Think of a large railway classification yard where every wagon is routed and checked before it rolls on. Every transfer gets caught.

## Status on devnet

Live on devnet as a reference deployment. Two programs are deployed and open to build against.

- `hukt_hooks` — [`4q7Tgd9A1XfTB2i6WLUjmFXNocw6GrshZwcKgarGV9aC`](https://explorer.solana.com/address/4q7Tgd9A1XfTB2i6WLUjmFXNocw6GrshZwcKgarGV9aC?cluster=devnet)
- `hukt_registry` — [`HkTcGxnRqmyBqrmMb63cad7sfJjzUo5jY4Y3ErQWBrGv`](https://explorer.solana.com/address/HkTcGxnRqmyBqrmMb63cad7sfJjzUo5jY4Y3ErQWBrGv?cluster=devnet)

Measured on chain so far: 11 transfers hooked, 1 blocked and reverted, 1 attested registry entry. Two packages are published on npm.

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#4A4E54','primaryTextColor':'#DDE1E4','primaryBorderColor':'#9AA0A6','lineColor':'#E8B23A','fontFamily':'Saira'}}}%%
flowchart LR
    T[Transfer] --> TK[Token-2022]
    TK -->|Execute CPI| H[Hook program]
    H -->|rules pass| V[Verified, settles]
    H -->|rules violated| X[Revert]
    R[Offchain resolver] -.->|extra accounts| T
    REG[Registry] -.->|attestation| H
```

## Repositories

- [hukt](https://github.com/HuktYard/hukt) — the framework monorepo: Anchor programs, shared Rust libraries, SDK, docs.
- [hukt-cli](https://github.com/HuktYard/hukt-cli) — command-line tool (`npm i -g hukt-cli`): inspect, resolve, attest, hook add, build.
- [hukt-resolver](https://github.com/HuktYard/hukt-resolver) — one-line TypeScript integration (`@hukt-labs/resolver`).

## How it works

1. Token-2022 issues an Execute CPI into the mint's hook program on every transfer.
2. The hook receives the extra accounts declared in its ExtraAccountMetaList and checks the transfer against its preset rules.
3. If a rule is violated the instruction reverts, so the transfer never settles.
4. An offchain resolver reconstructs those extra accounts for wallets and DEXs so a transfer can be built without knowing the hook internals.
5. The registry records an attestation for a hook so integrators can read its declared behavior before trusting it.

## Links

- Site: [hukt.fun](https://hukt.fun)
- X: [@huktfun](https://x.com/huktfun)
- npm: [hukt-cli](https://www.npmjs.com/package/hukt-cli), [@hukt-labs/resolver](https://www.npmjs.com/package/@hukt-labs/resolver)

CA: [CA]
