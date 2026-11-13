# clone-lin-pendle-syutils

Experimental **LIN** clone of Pendle V2 `SYUtils` — Standardized Yield ↔ underlying
asset conversion (`syToAsset`, `syToAssetUp`, `assetToSy`, `assetToSyUp`) with
128-bit floor and remainder-ceil muldiv.

**Proofs live in [kbelludoo/lin-open](https://github.com/kbelludoo/lin-open)** — this repo is the LIN copy only.

- **Upstream (hash-pinned, not vendored):** [pendle-finance/pendle-core-v2-public](https://github.com/pendle-finance/pendle-core-v2-public) `87685c89d05087535e9b9647eeda0e3d297d1f06`
  - `contracts/core/StandardizedYield/SYUtils.sol` sha256 `25af0a883e174b713a88296b97998bb323ab79356d65cb50f3c7adcef91158c3` blob `b6a5ed174c6b93c29654254a5ce10aab28178285`
  - License GPL-3.0-or-later. This repository does **not** vendor the Solidity source.
- **LIN kernel:** [`src/lin_pendle_syutils.lin`](src/lin_pendle_syutils.lin) (identical to lin-open)
- **C11 oracle copy:** [`test/oracles/pendle_syutils_c11.c`](test/oracles/pendle_syutils_c11.c)
- **Transpilable kernel:** [`test/oracles/pendle_syutils_kernel.c`](test/oracles/pendle_syutils_kernel.c)
- **Proofs (lin-open only):** `examples/pendle_syutils/` and `make pendle-syutils-proof`
- **Class:** EXPERIMENTAL uint64-scale. Not mainnet uint256. Not a production SY replacement.

Named goldens (`ONE=1e18`):

| id | call | value |
|---|---|---|
| ID | syToAsset(1e18, 1e18) | **1000000000000000000** |
| FLOOR | assetToSy(3e18, 10) | **3** |
| CEIL | assetToSyUp(3e18, 10) | **4** |
| DUST | syToAsset(3, 10) | **0** |
| DUST_UP | syToAssetUp(3, 10) | **1** |

Ceil is remainder-based (`floor+1` iff `r!=0`), not overflow-prone `(prod+d-1)/d`.
`exchangeRate==0` fail-closes. `1e18*1e18/1` fail-closes while naive i64 wraps.
`ONE`/`10**N` expansion and `CEIL_DIV` classification live in lin-open `src/lin_from_solidity.lin` v2.1.

## Reproduce proofs (in lin-open)

```
make pendle-syutils-proof
```

## Not claimed

Uint256 / mainnet Pendle SY / PT / YT parity.
LIN replacing Pendle, Uniswap, or the EVM. TVL or dollar savings. zk soundness.
That kernel `ONE'=1000` equals LIN `ONE=1e18`.
