# On-Chain Data URLs and ENS Hooks (EIP-8121)

In addition to distributed storage networks like IPFS/IPNS, Arweave/ArNS, and Swarm, eth.limo supports a storage backend where your content lives **directly on the blockchain**: on-chain data URLs, published through [EIP-8121](https://ethereum-magicians.org/t/erc-8121-delegated-metadata-resolution-via-hooks/27424) hooks.

Instead of pointing to a hash on a storage network, the ENS contenthash contains a **hook** — a fully self-describing smart contract call. When a compatible client (such as the eth.limo gateway) resolves the name, it executes that call and serves the returned data URL as the website content. No pinning services, no storage deals, no separate upload step — the chain itself is the storage layer.

## How It Works

An ENS contenthash normally encodes a storage protocol and an identifier (an IPFS CID, an Arweave transaction ID, a Swarm hash). With this backend the contenthash instead carries one of two payloads:

| Payload | Protocol code | Description |
| ------- | ------------- | ----------- |
| EIP-8121 hook | `0x30009b` | A smart contract call that returns the content |
| Plain data URI | `0x3000f2` | The data URL embedded directly in the contenthash |

A **hook** fully specifies what function to call, with what parameters, on which contract, on which chain:

- the **function signature** with explicit types (e.g. `data(bytes32)`)
- the **function call** with its values in human-readable form (e.g. `data(0x1234...)`)
- the **return type** for ABI decoding (`(bytes)` for content resolution)
- the **target** — an [ERC-7930](https://eips.ethereum.org/EIPS/eip-7930) interoperable address identifying both the contract *and* the chain it lives on

Because a hook is completely self-describing, any client can resolve it without external documentation. Resolution requires an [ERC-3668](https://eips.ethereum.org/EIPS/eip-3668) (CCIP-Read) enabled provider, which also makes cross-chain resolution possible: an ENS name on Ethereum mainnet can serve content from a contract on another EVM chain.

The resolution flow is:

1. Client reads the contenthash for the ENS name and detects the hook protocol code.
2. Client decodes the hook and (optionally) verifies the target contract against a trusted list.
3. Client executes the specified contract call on the specified chain — for example `data(nodehash)` on a `DataResolver` contract.
4. The returned bytes are decoded as a data URL (`data:text/html;base64,...`) and served as the website.

## When To Choose This Backend

| Consideration | On-chain Data URLs | Distributed storage (IPFS/Arweave/Swarm) |
| ------------- | ------------------ | ---------------------------------------- |
| Availability | As available as the chain's RPC layer — no pinning or gc | Depends on pinning, endowment, or postage stamps |
| Content size | Small artifacts only (~5KB is a practical target) | MBs to GBs |
| Cost | Gas for contract storage (expensive per byte) | Cheap or free per byte |
| Updates | Update contract state; contenthash can stay the same | New CID/transaction per revision (unless IPNS/ArNS) |
| Trust model | Verifiable contract call; client-side trust lists | Content addressing (hash verifies bytes) |
| Cross-chain | Yes — target any EIP-155 chain | N/A |

This makes on-chain data URLs a good fit for small, high-value artifacts: profile pages, on-chain "link in bio" microsites, credential and registry data, or metadata that other contracts need to reference — and a poor fit for media-heavy dWebsites, which should stay on IPFS, Arweave, or Swarm.

## Recommended Parameter Patterns

Hooks support functions with 0–2 parameters of fixed-size Solidity primitives:

- **0 parameters** — `getData()` — global/singleton data shared by all names pointing at the contract
- **1 parameter** — `data(bytes32 nodehash)` — per-name data keyed by the ENS nodehash
- **2 parameters** — `data(bytes32 nodehash, bytes32 hashOfContent)` — per-name data with cache-busting, where the second parameter is a hash of the expected content (integrity) or an autoincrement value (gateway cache invalidation)

## Trust Verification

One of the most important features of hooks is that clients can evaluate whether they trust the target contract *before* calling it. Resolvers can maintain trusted-target lists (per chain and address) and refuse or warn on hooks pointing at unknown contracts. This matters most when hooks are used for credential-style data (Proof-of-Personhood, KYC registries), but applies to content resolution too.

## Implementation Scope

The [ethlimo/ens-hooks](https://github.com/ethlimo/ens-hooks) library — used by the eth.limo gateway — is a focused implementation for contenthash resolution. Its intentional restrictions:

- 0–2 parameters of fixed-size primitives and strings (`bool`, `address`, `uintN`, `intN`, `bytesN`, `string` ≤ 512 chars); no dynamic `bytes`, arrays, structs, or tuples
- `bytes` return type only
- EIP-155 (EVM) chains only
- No recursive resolution — hooks cannot point to other hooks

See the library's [LIMITATIONS notes](https://github.com/ethlimo/ens-hooks/blob/main/notes/LIMITATIONS.md) for the full list and rationale.

## Publishing Content

Publishing is a three-step encoding flow, with tooling provided by the ens-hooks repository:

1. Artifact bytes → base64 **data URL** string
2. Hook metadata (target contract, chain, function) → **EIP-8121 hook bytes**
3. Hook bytes → **ENS contenthash bytes**

You then deploy (or reuse) a `DataResolver`-style contract, store the data URL on chain with `setData(node, dataUrl)`, and set the contenthash on your ENS name. Remember: on-chain storage is priced in gas, so keep artifacts to roughly 5KB or less.

For the complete step-by-step walkthrough — including the `encode-full` one-shot command and Sepolia testnet deployment — follow the [ens-hooks encoding guide](https://github.com/ethlimo/ens-hooks/blob/main/docs/guide.md).

## Resolving Content

The eth.limo gateway stack resolves hook-based contenthashes automatically through its Data URL server. To test end-to-end (including against testnets), the easiest path is [running a local gateway](../local-gateway/running-a-local-gateway.md), which ships with the Data URL server enabled by default.

## Resources

- [ens-hooks library](https://github.com/ethlimo/ens-hooks) — encoding, decoding, and execution (`@ethlimo/ens-hooks` on npm)
- [Encoding guide](https://github.com/ethlimo/ens-hooks/blob/main/docs/guide.md) — publish your first on-chain artifact
- [EIP-8121 discussion](https://ethereum-magicians.org/t/erc-8121-delegated-metadata-resolution-via-hooks/27424) — the hook specification
- [ERC-7930](https://eips.ethereum.org/EIPS/eip-7930) — interoperable addresses
- [ERC-3668](https://eips.ethereum.org/EIPS/eip-3668) — CCIP-Read
