---
description: Diagnosing and fixing common eth.limo resolution errors
---

# Troubleshooting

When eth.limo can't serve your dWebsite, it returns a branded error page. This guide explains each error, what causes it, and how to fix it — plus the diagnostic tools we recommend.

## Diagnostic Tools

Before anything else, these three lookups answer most questions:

| Tool | What it tells you |
| ---- | ----------------- |
| [ENS Manager App](https://app.ens.domains/) | The records currently set on your name — check the **Content Hash** field under Records |
| [Etherscan name lookup](https://etherscan.io/name-lookup) | On-chain view of your name's resolver and records |
| `https://dns.eth.limo/dns-query?name=<yourname.eth>` | What the eth.limo resolver itself sees — returns your decoded content hash as a `dnslink=` value |

The DoH endpoint is the fastest way to see your name exactly as the gateway does:

```bash
$ curl 'https://dns.eth.limo/dns-query?name=ens.eth'

{"Status":"0","TC":false,"Question":[{"name":"ens.eth","type":16}],"Answer":[{"name":"ens.eth","data":"dnslink=/ipfs/bafybei...","type":16,"ttl":300}]}
```

If the `Answer` contains the content hash you expect, resolution is working and the problem lies elsewhere (caching, or the content itself).

## Error: "ENS Domain Not Configured" (404)

**What it means:** your ENS name exists, but it has **no content hash record**. The gateway has nothing to serve.

**How to fix it:**

1. Upload your website to a supported storage network — see [Uploading to IPFS](beginner/how-to-use-ipfs-ipns/uploading-to-ipfs/README.md), [Hosting on Swarm](swarm/hosting-on-swarm.md), or [Arweave and ArNS](advanced/arweave-arns.md).
2. Copy the resulting content identifier (e.g. an IPFS CID like `bafy...`).
3. Open [app.ens.domains](https://app.ens.domains/), connect the wallet that manages the name, and select it.
4. Under **Records**, edit **Content Hash** and enter the identifier with its protocol prefix, e.g. `ipfs://bafy...`.
5. Save and confirm the transaction, then retry your site after a few minutes.

Full walkthrough: [Updating Your ENS Content Records](beginner/configuring-your-ens-name/updating-your-ens-content-records.md).

## Error: "Unsupported Contenthash" (422)

**What it means:** your name *has* a content hash record, but its value uses a protocol or encoding eth.limo can't resolve — the record is malformed, or points to an unsupported storage network.

**How to fix it:**

1. Inspect the current value with the [diagnostic tools](#diagnostic-tools) above.
2. Verify the value uses a supported protocol prefix:
   * `ipfs://` — IPFS CID
   * `ipns://` — IPNS record
   * `ar://` — Arweave transaction / ArNS name
   * `bzz://` — Swarm reference
   * `adnl://` — TON Site address
   * EIP-8121 hooks / data URLs — see [On-Chain Data URLs and ENS Hooks](advanced/onchain-data-urls.md)
3. Check for common encoding mistakes: pasting a gateway URL (`https://ipfs.io/ipfs/...`) instead of a `ipfs://` URI, truncated values, or the wrong record field entirely.
4. Update the record in the [ENS Manager](https://app.ens.domains/) and confirm the transaction.

If you believe the content hash *should* be supported, open a ticket in `#support-tickets` on our [Discord](https://discord.gg/zf8NxW94rB) (see [Still stuck?](#still-stuck) for the steps).

## My name resolves in my wallet but not on eth.limo

Check which network the name lives on. The gateway performs initial resolution on Ethereum Mainnet, Base (Basenames — `name.base.eth`), and Gnosis Mainnet (`.gno` names) — names on other chains or testnets won't resolve, even if wallets or explorers that support those networks display them. Records reached *via* CCIP-read from a resolver on a supported network work normally. See [Supported Networks](introduction/what-is-eth.limo/gateway-basics/README.md#supported-networks).

## Error: "Service Issue" (5xx)

**What it means:** the gateway hit an internal problem processing the request. This is usually temporary.

**What to do:**

1. Wait a moment and refresh.
2. Check [eth.limo](https://eth.limo) for known issues or maintenance.
3. If it persists, open a ticket in `#support-tickets` on our [Discord](https://discord.gg/zf8NxW94rB) and include the error code and status text shown at the bottom of the error page (see [Still stuck?](#still-stuck) for the steps).

## My record is correct but I still see old content

Resolution results are cached (the DoH `ttl` field shows 300 seconds). After updating a content hash, allow a few minutes for the change to propagate. IPNS and ArNS updates additionally depend on their own networks' propagation.

## My CID looks different after saving

Expected: CIDv0 values (`Qm...`) are automatically converted to CIDv1 (`bafy...`) when stored. Both refer to the same content — see [Understanding IPFS CIDs](beginner/configuring-your-ens-name/content-hash-overview/understanding-ipfs-content-identifiers-cids.md).

## My site loads but looks broken (missing styles/images)

Almost always absolute paths. Content served from a gateway must reference assets relatively (`./styles/main.css`), not absolutely (`/styles/main.css` or a hardcoded domain). Rebuild with relative paths and publish the new version.

## My site disappeared after working previously

Your content is no longer retrievable from its storage network:

* **IPFS** — the CID is no longer pinned anywhere. See [What is IPFS Pinning?](ipfs/what-is-ipfs-pinning.md)
* **Swarm** — the postage stamp batch expired. See [Hosting on Swarm](swarm/hosting-on-swarm.md)
* **IPNS** — the record expired and needs republishing. See [Running Your Own IPFS Node and IPNS Publishing](intermediate/running-your-own-ipfs-node.md)

## Still stuck?

For gateway-side issues, open a support ticket in our Discord:

1. Join the [eth.limo Discord](https://discord.gg/zf8NxW94rB).
2. Complete the server verification — interaction is not authorized until you're verified.
3. Open a ticket in the **`#support-tickets`** channel, including your ENS name and any error code shown.

For name and record management issues, use [ENS Support](https://support.ens.domains/).

To report phishing, malware, or other AUP violations, see [Acceptable Use & Abuse Reports](abuse.md).
