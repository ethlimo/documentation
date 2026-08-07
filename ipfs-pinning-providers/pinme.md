---
description: Using PinMe to publish and pin dWebsite content on IPFS
---

# PinMe

PinMe is a zero-config deployment CLI from [Glitter Protocol](https://glitterprotocol.io/) that publishes static sites to IPFS (backed by Filecoin) in a single command — no servers, SSL certificates, or infrastructure setup required. Every upload is pinned and returns a content identifier (CID) that you can use directly in your ENS content hash.

Fittingly, PinMe's own website is a dWebsite served through eth.limo: [https://pinme.eth.limo](https://pinme.eth.limo).

### Installation

Install the CLI globally via npm and verify it works:

```bash
npm install -g pinme
pinme --version
```

### Authentication

Uploads require authentication. Two options are available:

```bash
pinme login              # Browser-based login (recommended)
pinme set-appkey <key>   # Direct AppKey authentication (for CI/automated workflows)
```

Related commands:

```bash
pinme show-appkey        # Display masked credentials
pinme logout             # Sign out
```

### Next steps

* [PinMe IPFS Pinning](pinme-ipfs-pinning.md) — uploading files and directories, size limits, and connecting the resulting CID to your ENS name

### Resources

* Website: [https://pinme.eth.limo](https://pinme.eth.limo)
* Upload documentation: [https://pinme.eth.limo/upload-files.md](https://pinme.eth.limo/upload-files.md)
* GitHub: [https://github.com/glitternetwork/pinme](https://github.com/glitternetwork/pinme)
