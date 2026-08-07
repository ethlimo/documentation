# ENS Record Types: Contenthash, TXT Records, and Multiformats

This guide covers the various record types supported by ENS, with a focus on contenthash records, TXT records, and the multiformats specification that enables interoperability across different content addressing systems.

## Overview of ENS Record Types

ENS supports multiple record types that can be stored and resolved for each domain:

- **Address Records**: Ethereum and other cryptocurrency addresses
- **Contenthash Records**: Content addressing (IPFS, IPNS, Arweave, on-chain data URLs, etc.)
- **Text Records**: Key-value pairs for metadata
- **Custom Records**: Application-specific data

## Contenthash Records

Contenthash records are the primary mechanism for linking ENS domains to decentralized content, enabling dWebsites and other decentralized applications.

### Contenthash Format

Contenthash records use a standardized format that includes:
1. **Multicodec**: Identifies the content addressing system
2. **Content**: The actual content identifier

```
<multicodec><content>
```

### Supported Content Addressing Systems

Codec values per the [multicodec table](https://github.com/multiformats/multicodec/blob/master/table.csv):

| System | Multicodec | Example |
|--------|------------|---------|
| IPFS (`ipfs-ns`) | `0xe3` | `ipfs://bafy...` |
| Swarm (`swarm-ns`) | `0xe4` | `bzz://...` |
| IPNS (`ipns-ns`) | `0xe5` | `ipns://k51...` |
| Arweave (`arweave-ns`) | `0xb29910` | `ar://...` |
| TON (`adnl`) | `0xb69910` | `adnl://...` (experimental — see [TON Sites](../ton/ton-sites.md)) |
| EIP-8121 hook | `0x30009b` | On-chain contract call returning a data URL (draft, not yet in the multicodec table) |
| Data URI | `0x3000f2` | `data:...` embedded directly in the contenthash (draft, not yet in the multicodec table) |

For hook and data URI contenthashes, see [On-Chain Data URLs and ENS Hooks](onchain-data-urls.md).

Note that a contenthash value is not simply the codec byte concatenated with the raw identifier: per [ENSIP-7](https://docs.ens.domains/ensip/7), the protocol code is stored as an unsigned varint, and content-addressed systems like IPFS/IPNS append a full CID (version, content type, multihash). Always use a library rather than hand-encoding.

### Setting Contenthash Records

#### Using the ENS App

1. Visit [app.ens.domains](https://app.ens.domains)
2. Connect your wallet
3. Navigate to your domain
4. Click "Add/Edit Record"
5. Set the content hash to your IPFS/Arweave hash

#### Programmatically

Use the [`@ensdomains/content-hash`](https://github.com/ensdomains/content-hash) library for encoding and decoding — it implements ENSIP-7 correctly for all supported protocols:

```javascript
import { ethers } from 'ethers'; // ethers v6
import contentHash from '@ensdomains/content-hash';

// Encode a contenthash value
const encoded = '0x' + contentHash.encode('ipfs-ns', 'bafybeib...');   // IPFS CID
// contentHash.encode('ipns-ns', 'k51...')                             // IPNS
// contentHash.encode('swarm-ns', '<64-hex swarm reference>')          // Swarm
// contentHash.encode('arweave-ns', '<transaction id>')                // Arweave

// Set the record on your resolver
async function setContenthash(domain, encoded) {
    const resolver = new ethers.Contract(resolverAddress, RESOLVER_ABI, signer);
    const node = ethers.namehash(domain);
    await resolver.setContenthash(node, encoded);
}
```

#### Using IPFS

```bash
# Add content to IPFS
ipfs add -r /path/to/website

# Get the CID
CID=$(ipfs add -r -Q /path/to/website)

# Set contenthash (requires ENS resolver interaction)
# This would be done through a web interface or script
```

### Decoding Contenthash Records

The same library decodes stored values back to protocol and identifier:

```javascript
import contentHash from '@ensdomains/content-hash';

const encoded = await resolver.contenthash(ethers.namehash('ens.eth'));

contentHash.getCodec(encoded);  // e.g. 'ipfs-ns'
contentHash.decode(encoded);    // e.g. 'bafybeib...'
```

For a quick lookup without writing any code, the eth.limo DoH endpoint returns the decoded value directly:

```bash
curl 'https://dns.eth.limo/dns-query?name=ens.eth'
# ...,"data":"dnslink=/ipfs/bafybei...",...
```

## TXT Records

TXT records allow you to store arbitrary key-value pairs as metadata for your domain.

### Common TXT Record Keys

| Key | Purpose | Example |
|-----|---------|---------|
| `url` | Website URL | `https://example.com` |
| `email` | Contact email | `contact@example.com` |
| `description` | Domain description | `My personal website` |
| `avatar` | Profile picture | `ipfs://QmAvatar...` |
| `notice` | Legal notice | `Terms of service apply` |

### Setting TXT Records

```javascript
async function setTXTRecord(domain, key, value) {
    const resolver = new ethers.Contract(resolverAddress, RESOLVER_ABI, signer);
    const node = ethers.utils.namehash(domain);
    
    await resolver.setText(node, key, value);
}

// Examples
await setTXTRecord('mydomain.eth', 'url', 'https://mydomain.com');
await setTXTRecord('mydomain.eth', 'email', 'contact@mydomain.com');
await setTXTRecord('mydomain.eth', 'avatar', 'ipfs://QmAvatarHash');
```

### Reading TXT Records

```javascript
async function getTXTRecord(domain, key) {
    const resolver = new ethers.Contract(resolverAddress, RESOLVER_ABI, provider);
    const node = ethers.utils.namehash(domain);
    
    return await resolver.text(node, key);
}

// Examples
const url = await getTXTRecord('mydomain.eth', 'url');
const email = await getTXTRecord('mydomain.eth', 'email');
```

## Address Records

Address records map domain names to cryptocurrency addresses.

### Supported Address Types

```javascript
const ADDRESS_TYPES = {
    'ETH': 60,      // Ethereum
    'BTC': 0,       // Bitcoin
    'LTC': 2,       // Litecoin
    'DOGE': 3,      // Dogecoin
    'BCH': 145,     // Bitcoin Cash
    'XRP': 144,     // Ripple
    'ADA': 1815,    // Cardano
    'DOT': 354,     // Polkadot
    'LINK': 1977,   // Chainlink
    'UNI': 708,     // Uniswap
    'MATIC': 966    // Polygon
};
```

### Setting Address Records

```javascript
async function setAddressRecord(domain, coinType, address) {
    const resolver = new ethers.Contract(resolverAddress, RESOLVER_ABI, signer);
    const node = ethers.utils.namehash(domain);
    
    await resolver.setAddr(node, coinType, address);
}

// Examples
await setAddressRecord('mydomain.eth', 60, '0x1234...'); // ETH
await setAddressRecord('mydomain.eth', 0, '1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa'); // BTC
```

### Reading Address Records

```javascript
async function getAddressRecord(domain, coinType) {
    const resolver = new ethers.Contract(resolverAddress, RESOLVER_ABI, provider);
    const node = ethers.utils.namehash(domain);
    
    return await resolver.addr(node, coinType);
}

// Examples
const ethAddress = await getAddressRecord('mydomain.eth', 60);
const btcAddress = await getAddressRecord('mydomain.eth', 0);
```

## Advanced Record Patterns

### 1. Dynamic Content Routing

```javascript
class DynamicContentRouter {
    constructor(resolver) {
        this.resolver = resolver;
    }
    
    async setContentRoute(domain, path, contentHash) {
        const key = `content.${path}`;
        await this.resolver.setText(
            ethers.utils.namehash(domain),
            key,
            contentHash
        );
    }
    
    async getContentRoute(domain, path) {
        const key = `content.${path}`;
        return await this.resolver.text(
            ethers.utils.namehash(domain),
            key
        );
    }
}
```

### 2. Multi-Protocol Support

```javascript
class MultiProtocolResolver {
    async setMultiProtocolContent(domain, protocols) {
        const resolver = new ethers.Contract(resolverAddress, RESOLVER_ABI, signer);
        const node = ethers.utils.namehash(domain);
        
        // Set primary contenthash
        if (protocols.ipfs) {
            const encoded = MultiformatEncoder.encode('ipfs', protocols.ipfs);
            await resolver.setContenthash(node, encoded);
        }
        
        // Set fallback protocols as TXT records
        if (protocols.arweave) {
            await resolver.setText(node, 'arweave', protocols.arweave);
        }
        
        if (protocols.swarm) {
            await resolver.setText(node, 'swarm', protocols.swarm);
        }
    }
}
```

### 3. Versioned Content

```javascript
class VersionedContent {
    async setVersionedContent(domain, version, contentHash) {
        const resolver = new ethers.Contract(resolverAddress, RESOLVER_ABI, signer);
        const node = ethers.utils.namehash(domain);
        
        // Set version-specific content
        await resolver.setText(node, `version.${version}`, contentHash);
        
        // Update current version pointer
        await resolver.setText(node, 'current-version', version);
    }
    
    async getCurrentContent(domain) {
        const resolver = new ethers.Contract(resolverAddress, RESOLVER_ABI, provider);
        const node = ethers.utils.namehash(domain);
        
        const currentVersion = await resolver.text(node, 'current-version');
        return await resolver.text(node, `version.${currentVersion}`);
    }
}
```

## Best Practices

### 1. Content Hash Management

- **Use IPNS for mutable content**: Update content without changing ENS records
- **Set appropriate TTL**: Balance between freshness and gas costs
- **Implement fallbacks**: Use multiple content addressing systems

### 2. TXT Record Organization

- **Use consistent naming**: Follow established conventions
- **Keep records minimal**: Only store essential metadata
- **Document your schema**: Maintain a record of all TXT keys used

### 3. Address Record Security

- **Verify addresses**: Double-check all cryptocurrency addresses
- **Use hardware wallets**: Store private keys securely
- **Monitor for changes**: Track address record modifications

## Tools and Libraries

### JavaScript Libraries

```bash
npm install @ensdomains/ensjs
npm install multiformats
npm install @ipld/dag-cbor
```

### Example Usage

```javascript
import { ENS } from '@ensdomains/ensjs';
import { multiformats } from 'multiformats';

const ens = new ENS({ provider, chainId: 1 });

// Set contenthash
await ens.setContenthash('mydomain.eth', 'ipfs://QmHash');

// Get contenthash
const contenthash = await ens.getContenthash('mydomain.eth');

// Set TXT record
await ens.setText('mydomain.eth', 'url', 'https://example.com');

// Get TXT record
const url = await ens.getText('mydomain.eth', 'url');
```

## Troubleshooting

### Common Issues

**Contenthash not resolving:**
- Verify the multicodec is correct
- Check that the content exists on the network
- Ensure the resolver supports contenthash records

**TXT records not updating:**
- Check gas limits
- Verify resolver permissions
- Wait for transaction confirmation

**Address records showing wrong values:**
- Verify the coin type is correct
- Check for address format issues
- Ensure the resolver supports the coin type

## Resources

- [ENS Documentation](https://docs.ens.domains/)
- [Multiformats Specification](https://multiformats.io/)
- [IPFS Content Addressing](https://docs.ipfs.io/concepts/content-addressing/)
- [EIP-1577](https://eips.ethereum.org/EIPS/eip-1577) - Contenthash Specification

## Next Steps

With a solid understanding of record types, explore:

- [Alternatives to IPFS](alternatives-to-ipfs.md) - Other storage solutions
- [Arweave and ArNS](arweave-arns.md) - Permanent storage and naming
- [ENS Subdomains and CCIP](ens-subdomains-ccip.md) - Advanced subdomain management
