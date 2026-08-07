# Running a Local Gateway

Run the complete eth.limo gateway stack locally on your own machine — or share it with your home or work network. A local gateway lets you natively resolve ENS domains and dWebsite content **without relying on any third-party gateway**, with full feature parity with the public eth.limo service.

Because every storage protocol is accessed through its own light client, content is verified end-to-end. You choose the RPC provider, you control the network path (VPN/Tor friendly), and you keep full custody of your browsing metadata. Say goodbye to DNS hijacking, censorship and gateway outages.

## Features

| Feature | Description |
| ------- | ----------- |
| ✅ ENS resolution | Resolve ENS domains to dWeb content |
| ✅ IPFS & IPNS | Full IPFS gateway |
| ✅ Swarm | Full Swarm gateway |
| ✅ Arweave & ArNS | Full Arweave gateway |
| ✅ ENS enabled DNS-over-HTTPS (DoH) resolver | Extend ENS resolution to other applications |
| ✅ Origin isolation | Browser isolation enforced for all content |
| ✅ Secure HTTP headers by default | Safely enable all browser features |
| ✅ Per-site configuration | Configure settings on a per-site basis |
| ✅ Private | You control the network configuration; add a VPN or Tor for extra privacy |
| ✅ Full control | Customize every aspect of the gateway |
| ✅ Decentralized | Interact directly with each protocol — no middlemen |
| ✅ OS-level integration | Any HTTP client on your system can natively resolve ENS & dWeb content |

### Why isn't this a browser extension?

Browsers have enormous attack surfaces, and JavaScript sandboxing is best left to battle-tested browsers that have been fuzzed and hardened for decades. By running a local gateway you keep the security guarantees of the browser you already trust, while gaining native ENS/dWeb resolution — not just in the browser, but for *every* HTTP client on your system. This is especially attractive for agentic workloads and scrapers that need trustless ENS resolution without leaking traffic to third-party gateways.

## Requirements

### OS compatibility

| OS | Architecture |
| --- | --- |
| Linux | `amd64`, `arm64` |
| macOS | `arm64` |

### Container runtime

You need a container runtime and a compose implementation:

1. `docker` or `podman`
   * docker: [https://docs.docker.com/get-docker/](https://docs.docker.com/get-docker/)
   * podman: [https://podman.io/getting-started/installation](https://podman.io/getting-started/installation)
2. `docker-compose` or `podman-compose`
   * docker-compose: [https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)
   * podman-compose: [https://github.com/containers/podman-compose](https://github.com/containers/podman-compose)

The setup script auto-detects whichever runtime you have installed (rootless podman works too).

## Installation

```bash
git clone https://github.com/ethlimo/dweb-proxy-api.git
cd dweb-proxy-api/local_gateway
./setup.sh
```

The setup script will:

1. Start the gateway stack with your container runtime (`docker compose up -d` or the podman equivalent).
2. Wait for Caddy (the HTTP ingress) to generate a locally signed CA certificate.
3. Prompt you (via `sudo`) to install that CA certificate into your system trust store — the system keychain on macOS, or the trust anchor store on Linux.

The CA certificate is what enables HTTPS for `*.localhost` domains. Modern browsers require HTTPS for extended functionality, and without the certificate you would see "unknown issuer" warnings on every site.

{% hint style="info" %}
Caddy stores its certificates in a `./data` bind-mount directory created on first start. If you delete or regenerate this directory, re-run `./setup.sh` to reinstall the CA certificate.
{% endhint %}

### Day-to-day operation

Stop the gateway:

```bash
./stop.sh          # or: docker-compose down / podman-compose down
```

Start it again (after the initial setup):

```bash
./start.sh         # or: docker-compose up -d / podman-compose up -d
```

Check status:

```bash
docker ps          # or: podman ps
```

## Usage

Once the stack is running and the CA certificate is trusted, any ENS domain is reachable at `https://<name>.eth.localhost`:

* `ens.eth` → `https://ens.eth.localhost`
* `app.ens.eth` → `https://app.ens.eth.localhost`

The `.localhost` TLD is a reserved local namespace that most operating systems resolve without ever sending a DNS query to the network — resolution never leaves your machine.

And it isn't just for browsers. Any HTTP client can resolve dWebsites through your gateway:

```bash
curl https://ens.eth.localhost
```

This is perfect for scrapers, scripts and AI agents that need trustless ENS resolution.

### IPFS & IPNS

A full-featured IPFS gateway ([rainbow](https://github.com/ipfs/rainbow)) is included, with support for both IPFS and IPNS:

| Example | Description |
| ------- | ----------- |
| `https://{cid}.ipfs.localhost` | IPFS CID |
| `https://{ipns_record}.ipns.localhost` | IPNS record |
| `https://ens-eth.ipns.localhost` | ENS domain resolving to IPFS content (domain labels must be flattened: `ens-eth`, not `ens.eth`) |

### Arweave & ArNS

An Arweave light-client gateway ([wayfinder-router](https://github.com/vilenarios/wayfinder-router)) is included, with support for Arweave transactions and ArNS records:

| Example | Description |
| ------- | ----------- |
| `https://{tx}.arweave.localhost` | Arweave transaction |
| `https://{sandbox_subdomain}.arweave.localhost/{tx}` | Sandbox format |

### Swarm

A Swarm light-client gateway ([bee](https://github.com/ethersphere/bee)) is included, with support for Swarm identifiers and ENS domains that resolve to Swarm content:

| Example | Description |
| ------- | ----------- |
| `https://{swarm_cid}.swarm.localhost` | Swarm hash |

### DNS-over-HTTPS

Just like the public [eth.limo DoH resolver](../dns-over-https/doh.md), your local gateway exposes a DoH endpoint that extends native ENS resolution to any application supporting DNS-over-HTTPS. It also doubles as a handy lookup tool for decoded content hash records:

```bash
$ curl 'https://dns.eth.localhost/dns-query?name=ens.eth'

{"Status":"0","TC":false,"Question":[{"name":"ens.eth","type":16}],"Answer":[{"name":"ens.eth","data":"dnslink=/ipfs/bafybeifnx3u22ngv4ygpnj32qkwzrpgizw4i7e3swp4v6am5piiih3ude4","type":16,"ttl":300}]}
```

## Under the hood

The stack is defined in a single `docker-compose.yml` and consists of:

| Service | Role |
| ------- | ---- |
| `ingress` (Caddy) | HTTPS termination, routing, security headers, origin isolation |
| `dweb-proxy-api` | ENS/GNS resolution, DoH API, content hash decoding |
| `redis` | Resolution cache |
| `rainbow` | IPFS/IPNS gateway |
| `wayfinder` | Arweave/ArNS gateway |
| `bee` | Swarm gateway |
| `wildcard-dns` (dnsmasq) | Wildcard DNS inside the container network |

All containers run with dropped capabilities, read-only filesystems and resource limits by default.

## Configuration

Common settings can be overridden with environment variables — either exported in your shell before starting the stack, or set directly in `docker-compose.yml`:

| Variable | Default | Description |
| -------- | ------- | ----------- |
| `ETH_RPC_ENDPOINT` | `https://ethereum.publicnode.com` | Ethereum (mainnet) RPC endpoint |
| `ETH_CHAIN_ID` | `1` | EIP-155 chain ID; set to `11155111` when pointing at Sepolia |
| `GNO_RPC_ENDPOINT` | `https://rpc.gnosischain.com` | Gnosis Chain RPC endpoint |
| `HTTP_PORT` / `HTTPS_PORT` | `80` / `443` | Host ports the gateway listens on |
| `CACHE_TTL` | `300` | How long (seconds) domain resolution results are cached |
| `RAINBOW_HTTP_ROUTERS` | `https://delegated-ipfs.dev,https://indexer.pinata.cloud,https://cid.contact` | Delegated routing endpoints used by the IPFS gateway |

For example, to point the gateway at your own node or preferred RPC service:

```bash
export ETH_RPC_ENDPOINT="https://<your-rpc-here>"
export GNO_RPC_ENDPOINT="https://<your-rpc-here>"
./start.sh
```

### Customizing sites and headers

The HTTP ingress is powered by [Caddy](https://caddyserver.com/), and every Caddy configuration file lives under `./caddy` — per-protocol sites in `./caddy/sites`, shared routes in `./caddy/routes`, reusable snippets in `./caddy/snippets`. The most common customization is adding or modifying headers for a specific site. For example, edit `./caddy/snippets/headers.Caddyfile`:

```
	# Site-specific headers can be added here if needed
	# Example: enable COOP and COEP for WASM applications

	# COOP and COEP headers
	@WasmHeaders {
		expression {host}.contains("domain.eth")
	}

	header @WasmHeaders {
		Cross-Origin-Embedder-Policy "credentialless"
		Cross-Origin-Opener-Policy "same-origin"
	}
```

Save the file and restart the gateway stack for the change to take effect.

### Data URLs & EIP-8121 hooks (on-chain content)

Besides pointing to distributed storage (IPFS, Arweave, Swarm), an ENS content hash can contain an [EIP-8121 hook](https://github.com/ethlimo/ens-hooks): a payload that fully specifies a smart contract call — which function to invoke, with what parameters, on which contract, on which chain. Instead of fetching from a storage network, the gateway executes that call and serves the returned **data URL** — your content comes straight from the blockchain.

Publishing content this way involves three encoding steps (handled by the [ens-hooks](https://github.com/ethlimo/ens-hooks) tooling):

1. Artifact bytes → base64 data URL string
2. Hook metadata → EIP-8121 hook bytes
3. Hook bytes → ENS contenthash bytes

Because on-chain storage is expensive, keep artifacts small — about 5KB or less is a practical target.

The local gateway's Data URL server resolves these hooks out of the box: it decodes the hook payload from the ENS content hash, executes the contract call via ethers.js, and returns the result. It is enabled by default (`DATAURL_ENABLED=true`) and serves requests at `GET /api/v1/dataurl/:ensname/:payload`. If you see 500 errors, check that `DATAURL_ENDPOINT` is set and reachable.

For a complete walkthrough — encoding an artifact as a data URL, deploying a `DataResolver` contract, and publishing the contenthash on Sepolia testnet — see the [ens-hooks encoding guide](https://github.com/ethlimo/ens-hooks/blob/main/docs/guide.md). A running local gateway (this page) is a prerequisite for that guide; you'll simply point it at Sepolia by setting `ETH_RPC_ENDPOINT` to a Sepolia RPC endpoint and `ETH_CHAIN_ID=11155111` before starting the stack.

## Privacy & security considerations

Local resolution is a big privacy improvement over public gateways, but keep the following in mind:

1. **Network configuration** — By default the stack makes outbound connections over your default network interface: RPC calls, IPFS & Swarm peer connections, CCIP-read resolvers, Arweave trustless gateways, third-party embeds, CDNs and more. Your public IP is visible to every peer you connect to. Route traffic through a trusted VPN or Tor if that matters to you.
2. **RPC provider** — ENS/GNS resolution requires reading from the Ethereum and Gnosis blockchains. For maximum privacy, run your own full node or choose an RPC provider that respects user privacy (see [Configuration](#configuration) above).
3. **Malicious content** — A trustless transport does not make the content itself trustworthy. Exercise caution on unfamiliar sites, especially anything that asks you to connect a wallet. Never share private keys or seed phrases with any site, ever.

## Troubleshooting

* **"Unknown issuer" / certificate warnings** — the local CA certificate is not (or no longer) in your system trust store. Re-run `./setup.sh`, which reinstalls it.
* **HTTPS breaks after deleting `./data`** — the CA certificate is regenerated with the directory; re-run `./setup.sh` to trust the new one.
* **Port conflict on 80/443** — another service is already bound to those ports. Set `HTTP_PORT`/`HTTPS_PORT` to alternative values before starting.
* **`.localhost` doesn't resolve** — most operating systems resolve `*.localhost` to the loopback address automatically, but a few DNS setups interfere. Check with `ping test.eth.localhost`.
