# DEUS Solana OFT — Verified Source

This repository contains the minimal source code required to reproduce a deterministic build of the DEUS OFT (Omnichain Fungible Token) program deployed on Solana. It exists solely for on-chain source verification via [solana-verify](https://github.com/Ellipsis-Labs/solana-verifiable-build).

## Program Details

| Field | Value |
|-------|-------|
| **Program ID** | `FNApPiNRrsA8k9jgqh4W9BqWCShDj6oRMUi8yMzBxcNw` |
| **Program Hash** | `206691c6146b76abc8cbf4d79317ffbf3d63cef1ce2f16daec5cfe2de5198312` |
| **Solana Version** | 1.17.31 |
| **Anchor Version** | 0.29.0 |
| **Rust Toolchain** | 1.75.0 |
| **Verification TX** | `3rzcwh4iB2EY14y4Muveb1JR5aUvnCJznVP5JGfCqoYJ9q9p47a4w6uzHeMLgWtzkMP1YDUV6QeUGmuw53PLrycU` |

## Verified On

- [Solana Explorer](https://explorer.solana.com/address/FNApPiNRrsA8k9jgqh4W9BqWCShDj6oRMUi8yMzBxcNw)
- [Solscan](https://solscan.io/account/FNApPiNRrsA8k9jgqh4W9BqWCShDj6oRMUi8yMzBxcNw)

## What Is This?

The DEUS token uses [LayerZero's OFT standard](https://docs.layerzero.network/v2) to bridge across chains. The Solana program is a standard LayerZero OFT built with Anchor. This repo proves that the deployed on-chain bytecode was built from exactly this source code — no modifications, no hidden logic.

## Reproducing the Build

Anyone can independently verify the program hash matches the on-chain deployment:

```bash
# Install solana-verify
cargo install solana-verify

# Build deterministically in Docker and compare against on-chain program
solana-verify verify-from-repo \
  -um \
  --program-id FNApPiNRrsA8k9jgqh4W9BqWCShDj6oRMUi8yMzBxcNw \
  --library-name oft \
  https://github.com/XMAQUINA-DAO/deus-solana-oft
```

## Repository Contents

This is a minimal subset of the full project — only the files needed for a reproducible build:

```
├── Cargo.toml              # Workspace definition
├── Cargo.lock              # Locked dependencies (critical for reproducibility)
├── Anchor.toml             # Anchor config with program ID
├── rust-toolchain.toml     # Pins Rust 1.75.0
└── programs/
    ├── oft/                # The OFT program source
    └── endpoint-mock/      # Workspace member (required for Cargo.lock resolution)
```

The `endpoint-mock` program is included because the workspace `Cargo.toml` uses `members = ["programs/*"]`. Removing it would alter the Cargo.lock resolution and break hash reproducibility.
