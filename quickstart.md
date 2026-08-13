---
description: From nothing to a live dWebsite in five steps
---

# Quickstart: Launch Your dWebsite

This is the fastest path from zero to a live decentralized website at `yourname.eth.limo`. Each step links to a detailed guide if you need more depth.

### What you'll need

* An Ethereum wallet (e.g. MetaMask) with a small amount of ETH for registration and one record-update transaction
* A static website — plain HTML/CSS/JS or the build output of any static site generator

### Step 1: Register an ENS name

Go to [app.ens.domains](https://app.ens.domains/), search for an available name, and complete the three-step registration (commit → wait 60 seconds → register).

📖 Details: [How to register an ENS domain](beginner/how-to-register-an-ens-domain.md)

### Step 2: Prepare your site

One rule matters more than any other: **use relative paths** for all assets (`./styles/main.css`, not `/styles/main.css`). Content-addressed hosting serves your site from different URL roots, and absolute paths will break. Compress images and minify where you can.

### Step 3: Publish to decentralized storage

The quickest option is an IPFS pinning service — upload your site folder and copy the resulting CID (`bafy...`):

* [PinMe](ipfs-pinning-providers/pinme.md) — one CLI command: `pinme upload ./dist`
* [Pinata](ipfs-pinning-providers/pinata.md), [Filebase](ipfs-pinning-providers/filebase.md), or [4Everland](ipfs-pinning-providers/4everland.md) — web dashboards
* Prefer permanence or Ethereum-native storage? [Arweave](advanced/arweave-arns.md) or [Swarm](swarm/hosting-on-swarm.md)

📖 Why pinning matters: [What is IPFS Pinning?](ipfs/what-is-ipfs-pinning.md)

### Step 4: Set your content hash

In the [ENS Manager](https://app.ens.domains/), open your name → **Records** → edit **Content Hash**, and enter your identifier with its protocol prefix:

```
ipfs://bafybeib...
```

(Use `ipns://`, `ar://`, `bzz://`, or `adnl://` for other protocols.) Save and confirm the transaction.

📖 Details: [Updating Your ENS Content Records](beginner/configuring-your-ens-name/updating-your-ens-content-records.md)

### Step 5: Visit your site

Open `https://yourname.eth.limo` in any browser. Allow up to 5 minutes for the resolution cache after record changes.

You can verify what the gateway sees at any time:

```bash
curl 'https://dns.eth.limo/dns-query?name=yourname.eth'
```

### Something not working?

See the [Troubleshooting](troubleshooting.md) guide — it covers every error page the gateway returns and the most common configuration mistakes.

### Where to go next

* Update content without new transactions: [How to Publish to IPNS](ipns-publishing/how-to-publish-to-ipns.md)
* Own your whole stack: [Running a Local Gateway](local-gateway/running-a-local-gateway.md)
* Fully on-chain microsites: [On-Chain Data URLs and ENS Hooks](advanced/onchain-data-urls.md)
