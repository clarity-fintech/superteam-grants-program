# Grants Program

> **Moniversive Invariant Static (MIS)** module · settles on **CLRTY-1** (chain **1202**) · compiler **`misc`**  
> Creator **Chandler William Ferguson** · Org [`clarity-fintech/superteam-grants-program`](https://github.com/clarity-fintech/superteam-grants-program)

MIS-only (`.mis`). Compiler `bin/misc`. Settlement **clrty-1 / chain 1202**. Embed gates **3..=6**.

This repository is **self-contained and 100% downloadable**: it ships the prebuilt MIS
compiler at [`bin/misc`](bin/misc), so a fresh clone builds and checks with no extra setup.

---

## Contents

- [What this is](#what-this-is)
- [Quickstart](#quickstart)
- [Command reference](#command-reference)
- [Architecture & layout](#architecture--layout)
- [Kernel & invariants](#kernel--invariants)
- [Code sample — an invariant pack](#code-sample--an-invariant-pack)
- [Network binding (CLRTY-1)](#network-binding-clrty-1)
- [Verification / test output](#verification--test-output)
- [Bootstrap & portability](#bootstrap--portability)
- [Backlinks](#backlinks)

## What this is

**Superteam Grants Program** is written entirely in **MIS** (Moniversive Invariant Static, `.mis`) — a
letter-hashed, invariant-static language. Every module compiles under the **`misc`** kernel
(foreign kernels are rejected) to a deterministic *typed-letter root*, so a check is a
cryptographic proof that the module's invariants hold.

- **26** `.mis` modules: **1** kernel · **5** packs · **17** sections · **3** command modules
- **9** active invariants and **4** outcomes in the backlink kernel (verified below)
- Settlement network **CLRTY-1 / chain 1202**, RPC `https://rpc.clarity-fintech.com`
- Embed gates **3..=6**; deep root anchored to `moniversive`

## Quickstart

```bash
git clone https://github.com/clarity-fintech/superteam-grants-program
cd superteam-grants-program

# 0. (optional) refresh / rebuild the compiler — a prebuilt bin/misc is already committed
make bootstrap

# 1. compile-check the kernel + every .mis module in the repo
make check

# 2. validate the command surface
make commands-check

# 3. print the live CLRTY-1 network binding
make network-connect
```

## Command reference

| Command | What it does |
|---|---|
| `make bootstrap` | Ensure/refresh `bin/misc` (copies a shared build, or rebuilds from the CLRTY-MIS-Kernel Rust source). |
| `make check` | Compile-check the kernel and **every** `.mis` file with `--compact-letters`; prints the kernel proof JSON. |
| `make commands-check` | Compile-check only the command catalog under `mis/commands/`. |
| `make network-connect` | Runs `commands-check`, then prints the CLRTY-1 (chain 1202) binding, RPC, and command module. |
| `bin/misc mis/kernel/SuperteamGrantsProgramKernel.mis --check --compact-letters` | Check a single module directly with the compiler. |

Direct compiler invocation (what the Makefile runs under the hood):

```bash
bin/misc mis/kernel/SuperteamGrantsProgramKernel.mis --check --compact-letters
find mis -name '*.mis' -print0 | xargs -0 -n1 bin/misc --check --compact-letters
```

## Architecture & layout

```
mis/kernel/SuperteamGrantsProgramKernel.mis
mis/packs/audit_trail.mis
mis/packs/grant_track.mis
mis/packs/milestone_escrow.mis
mis/packs/release_gate.mis
mis/packs/treasury_dist.mis
mis/sections/ClrtyMisKernelRepo.mis
mis/sections/GradientBand.mis
mis/sections/LanguageRoot.mis
mis/sections/MisAllCodeIndex.mis
mis/sections/MisArchitecture.mis
mis/sections/MisChainIntake.mis
mis/sections/MisCodeIndex.mis
mis/sections/MisEcosystemRepos.mis
mis/sections/MisKernelActiveOnly.mis
mis/sections/MisKernelSource.mis
mis/sections/MisNativeKernels.mis
mis/sections/MiscCompiler.mis
mis/sections/MlKernelRepo.mis
mis/sections/ProtocolStructures.mis
mis/sections/StaticMlCatalog.mis
mis/sections/TickArchitecture.mis
mis/sections/TickStakingGradient.mis
mis/commands/SuperteamGrantsProgramCommandCatalog.mis
mis/commands/SuperteamGrantsProgramCommands.mis
mis/commands/SuperteamGrantsProgramNetworkBind.mis
manifests/commands_manifest.json
manifests/domain_full_index.json
manifests/mis_repo_manifest.json
```

- **`mis/kernel/`** — the module's kernel entry: invariants + outcomes that gate everything else.
- **`mis/packs/`** — invariant packs (the executable logic units).
- **`mis/sections/`** — MIS + CLRTY-1 + edge backlink sections binding this repo into the ecosystem.
- **`mis/commands/`** — the callable command catalog and its CLRTY-1 network binding.
- **`manifests/`** — machine-readable domain, command, and repo manifests.

## Kernel & invariants

`mis/kernel/SuperteamGrantsProgramKernel.mis` — the kernel declares the invariants the compiler enforces on every check:

```rust
// Moniversive Invariant Static (MIS) — deep root
// Creator: Chandler William Ferguson
// Compile: bin/misc <file>.mis --check --compact-letters
// Title: Superteam Grants Program Kernel

module SuperteamGrantsProgramKernel {
  invariant letter_hash_bound: letter_hash_root != @0;
  invariant deep_root_moniversive: deep_root == moniversive;
  invariant settlement_chain: chain_id == 1202;
  invariant settlement_network_clrty1: settlement_network == clrty_1;
  invariant extension_mis: source_extension == mis;
  invariant kernel_is_misc: compiler_kernel == misc;
  invariant active_kernel_only: active_kernel == misc;
  invariant no_foreign_kernel: foreign_kernel_active == false;
  invariant creator_bound: creator == chandler_william_ferguson;
  invariant domain_bound: domain == superteam_grants;
  invariant clrty1_architecture_bound: clrty1_architecture_bound == true;
  invariant ecosystem_catalog_bound: ecosystem_catalog_bound == true;

  invariant pack_grant_track_flag: pack_grant_track_bound == true;
  invariant pack_milestone_escrow_flag: pack_milestone_escrow_bound == true;
  invariant pack_treasury_dist_flag: pack_treasury_dist_bound == true;
  invariant pack_release_gate_flag: pack_release_gate_bound == true;
  invariant pack_audit_trail_flag: pack_audit_trail_bound == true;

  outcome assert_kernel_misc() {
```

## Code sample — an invariant pack

`mis/packs/audit_trail.mis`:

```rust
// Moniversive Invariant Static (MIS) — deep root
// Creator: Chandler William Ferguson
// Compile: bin/misc <file>.mis --check --compact-letters
// Title: Superteam Grants Program · AuditTrail

module SuperteamGrantsProgramAuditTrail {
  invariant letter_hash_bound: letter_hash_root != @0;
  invariant deep_root_moniversive: deep_root == moniversive;
  invariant settlement_chain: chain_id == 1202;
  invariant settlement_network_clrty1: settlement_network == clrty_1;
  invariant kernel_is_misc: compiler_kernel == misc;
  invariant pack_audit_trail_bound: pack_bound == true;
  invariant catalog_backlink: mis_catalog_bound == true;

  invariant audit_trail_081: audit_trail_081_bound == true;
  invariant audit_trail_082: audit_trail_082_bound == true;
  invariant audit_trail_083: audit_trail_083_bound == true;
  invariant audit_trail_084: audit_trail_084_bound == true;
  invariant audit_trail_085: audit_trail_085_bound == true;
  invariant audit_trail_086: audit_trail_086_bound == true;
  invariant audit_trail_087: audit_trail_087_bound == true;
  invariant audit_trail_088: audit_trail_088_bound == true;
```

## Network binding (CLRTY-1)

`make network-connect` binds the command surface to the settlement network:

```
CONNECTED clrty-1/1202 superteam-grants-program
RPC https://rpc.clarity-fintech.com
MODULES mis/commands/SuperteamGrantsProgramCommands.mis
```

## Verification / test output

`make check` passes — the kernel proof for this repo:

```json
{
  "ok": true,
  "module": "SuperteamGrantsProgramBacklinkIndex",
  "kernel": "misc",
  "invariant_count": 9,
  "outcome_count": 4,
  "typed_letters": 1171,
  "active_kernel_only": true
}
```

*Reproduce:* `make check` → `"ok": true`, module `SuperteamGrantsProgramBacklinkIndex`, 9 invariants, 4 outcomes, 1171 typed letters.

## Bootstrap & portability

- A **prebuilt macOS `bin/misc`** is committed, so `make check` works immediately after clone.
- On other platforms (or to rebuild), run **`make bootstrap`** — it rebuilds `misc` from the
  [CLRTY-MIS-Kernel](https://github.com/clarity-fintech/CLRTY-MIS-Kernel) Rust source via `cargo`,
  or copies a shared build if one is present in a parent checkout.
- The compiler accepts **MIS only**; foreign kernels are rejected by design.

## Backlinks

- [clarity-fintech/CLRTY-MIS-Kernel](https://github.com/clarity-fintech/CLRTY-MIS-Kernel) — the sole active `misc` compiler
- [clarity-fintech/moniversive_invariant_static_ML](https://github.com/clarity-fintech/moniversive_invariant_static_ML) — MIS language root
- [clarity-fintech/superteam-grants-program](https://github.com/clarity-fintech/superteam-grants-program) — this repository

---
MIS · CLRTY-1 (chain 1202) · Creator Chandler William Ferguson
