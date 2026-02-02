# ENS Record Types: Contenthash, TXT Records, and Multiformats

This guide covers the various record types supported by ENS, with a focus on contenthash records, TXT records, and the multiformats specification that enables interoperability across different content addressing systems.

## Overview of ENS Record Types

ENS supports multiple record types that can be stored and resolved for each domain:

- **Address Records**: Ethereum and other cryptocurrency addresses
- **Contenthash Records**: Content addressing (IPFS, IPNS, Arweave, etc.)
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

| System | Multicodec | Example |
|--------|------------|---------|
| IPFS | `0xe3` | `ipfs://Qm...` |
| IPNS | `0xe5` | `ipns://k51...` |
| Arweave | `0x6b` | `ar://...` |
| Swarm | `0x7b` | `bzz://...` |

### Setting Contenthash Records

#### Using the ENS App

1. Visit [app.ens.domains](https://app.ens.domains)
2. Connect your wallet
3. Navigate to your domain
4. Click "Add/Edit Record"
5. Set the content hash to your IPFS/Arweave hash

#### Using Ethers.js

```javascript
const { ethers } = require('ethers');

// Encode contenthash for IPFS
function encodeContenthash(content) {
    if (content.startsWith('ipfs://')) {
        const hash = content.replace('ipfs://', '');
        const bytes = ethers.utils.base58decode(hash);
        return ethers.utils.hexlify(ethers.utils.concat(['0xe3', bytes]));
    }
    
    if (content.startsWith('ipns://')) {
        const hash = content.replace('ipns://', '');
        const bytes = ethers.utils.base58decode(hash);
        return ethers.utils.hexlify(ethers.utils.concat(['0xe5', bytes]));
    }
    
    throw new Error('Unsupported content addressing system');
}

// Set contenthash record
async function setContenthash(domain, content) {
    const resolver = new ethers.Contract(resolverAddress, RESOLVER_ABI, signer);
    const node = ethers.utils.namehash(domain);
    const encoded = encodeContenthash(content);
    
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

```javascript
function decodeContenthash(encoded) {
    const bytes = ethers.utils.arrayify(encoded);
    
    if (bytes.length === 0) {
        return null;
    }
    
    const multicodec = bytes[0];
    const content = bytes.slice(1);
    
    switch (multicodec) {
        case 0xe3: // IPFS
            const ipfsHash = ethers.utils.base58encode(content);
            return `ipfs://${ipfsHash}`;
            
        case 0xe5: // IPNS
            const ipnsHash = ethers.utils.base58encode(content);
            return `ipns://${ipnsHash}`;
            
        case 0x6b: // Arweave
            const arweaveId = ethers.utils.toUtf8String(content);
            return `ar://${arweaveId}`;
            
        default:
            throw new Error(`Unknown multicodec: ${multicodec}`);
    }
}
```

## Multiformats and Multicodecs

Multiformats is a self-describing data format that enables interoperability between different content addressing systems.

### Multicodec Table

```javascript
const MULTICODECS = {
    // IPFS
    'ipfs': 0xe3,
    'ipfs-ns': 0xe5,
    
    // Arweave
    'arweave': 0x6b,
    
    // Swarm
    'swarm': 0x7b,
    
    // Other systems
    'skynet': 0x1b,
    'sia': 0x1c
};

// Note: Verify multicodec values with current multiformats specification
// https://github.com/multiformats/multicodec/blob/master/table.csv
```

### Multiformat Encoding

```javascript
class MultiformatEncoder {
    static encode(system, content) {
        const multicodec = MULTICODECS[system];
        if (!multicodec) {
            throw new Error(`Unknown system: ${system}`);
        }
        
        let encodedContent;
        switch (system) {
            case 'ipfs':
            case 'ipfs-ns':
                encodedContent = ethers.utils.base58decode(content);
                break;
            case 'arweave':
                encodedContent = ethers.utils.toUtf8Bytes(content);
                break;
            default:
                throw new Error(`Unsupported system: ${system}`);
        }
        
        return ethers.utils.hexlify(
            ethers.utils.concat([multicodec, encodedContent])
        );
    }
    
    static decode(encoded) {
        const bytes = ethers.utils.arrayify(encoded);
        const multicodec = bytes[0];
        const content = bytes.slice(1);
        
        // Find system by multicodec
        const system = Object.keys(MULTICODECS).find(
            key => MULTICODECS[key] === multicodec
        );
        
        if (!system) {
            throw new Error(`Unknown multicodec: ${multicodec}`);
        }
        
        let decodedContent;
        switch (system) {
            case 'ipfs':
            case 'ipfs-ns':
                decodedContent = ethers.utils.base58encode(content);
                break;
            case 'arweave':
                decodedContent = ethers.utils.toUtf8String(content);
                break;
            default:
                throw new Error(`Unsupported system: ${system}`);
        }
        
        return {
            system,
            content: decodedContent,
            full: `${system}://${decodedContent}`
        };
    }
}
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
