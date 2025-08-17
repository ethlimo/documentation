# ENS Subdomains and CCIP (Cross-Chain Interoperability Protocol)

This guide covers advanced ENS subdomain management and the Cross-Chain Interoperability Protocol (CCIP) for creating sophisticated dWebsite architectures with wildcard resolvers.

## Understanding ENS Subdomains

ENS subdomains allow you to create hierarchical naming structures under your main domain. For example, if you own `mydomain.eth`, you can create subdomains like:
- `blog.mydomain.eth`
- `docs.mydomain.eth`
- `api.mydomain.eth`

### Types of Subdomains

1. **Static Subdomains**: Manually created and managed
2. **Dynamic Subdomains**: Programmatically generated
3. **Wildcard Subdomains**: Catch-all subdomains using CCIP

## Creating Static Subdomains

### Using the ENS App

1. Visit [app.ens.domains](https://app.ens.domains)
2. Connect your wallet
3. Navigate to your domain
4. Click "Add/Edit Record"
5. Add a subdomain with desired records

### Using Etherscan

```javascript
// Example: Creating a subdomain via contract interaction
const ensRegistry = new ethers.Contract(ENS_REGISTRY_ADDRESS, ENS_ABI, signer);

await ensRegistry.setSubnodeRecord(
  ethers.utils.namehash('mydomain.eth'),
  ethers.utils.keccak256(ethers.utils.toUtf8Bytes('blog')),
  ownerAddress,
  resolverAddress,
  0 // TTL
);
```

## Cross-Chain Interoperability Protocol (CCIP)

CCIP enables ENS domains to resolve across different blockchains and networks, making it possible to create truly decentralized and cross-chain dWebsites.

### How CCIP Works

1. **Off-Chain Resolution**: CCIP allows resolvers to fetch data from off-chain sources
2. **Cross-Chain Data**: Resolvers can pull data from other blockchains
3. **Dynamic Content**: Content can be updated without on-chain transactions

### CCIP-Read Specification

CCIP-Read enables off-chain data resolution:

```solidity
interface IExtendedResolver {
    function resolve(bytes calldata name, bytes calldata data) 
        external view returns (bytes memory result, uint64 validUntil, address resolver);
}
```

## Wildcard Resolvers

Wildcard resolvers allow you to handle any subdomain dynamically without creating individual records for each one.

### Setting Up a Wildcard Resolver

```solidity
// Example wildcard resolver contract
contract WildcardResolver {
    mapping(bytes32 => address) public owners;
    
    function resolve(bytes calldata name, bytes calldata data) 
        external view returns (bytes memory result, uint64 validUntil, address resolver) {
        
        // Parse the name to extract subdomain
        string memory subdomain = extractSubdomain(name);
        
        // Return appropriate content based on subdomain
        if (keccak256(abi.encodePacked(subdomain)) == keccak256(abi.encodePacked("blog"))) {
            return (abi.encode("ipfs://QmBlogContent"), block.timestamp + 1 days, address(this));
        } else if (keccak256(abi.encodePacked(subdomain)) == keccak256(abi.encodePacked("docs"))) {
            return (abi.encode("ipfs://QmDocsContent"), block.timestamp + 1 days, address(this));
        }
        
        // Default fallback
        return (abi.encode("ipfs://QmDefaultContent"), block.timestamp + 1 days, address(this));
    }
}
```

### Popular Wildcard Resolver Implementations

#### 1. NameStone

NameStone provides a comprehensive wildcard resolver solution:

```javascript
// Using NameStone for wildcard resolution
const namestone = new NameStone({
  domain: 'mydomain.eth',
  resolver: '0x...' // NameStone resolver address
});

// Configure subdomain routing
await namestone.setSubdomainRoute('blog', 'ipfs://QmBlogContent');
await namestone.setSubdomainRoute('docs', 'ipfs://QmDocsContent');
await namestone.setSubdomainRoute('*', 'ipfs://QmDefaultContent'); // Wildcard
```

#### 2. NameSys

NameSys offers advanced subdomain management:

```javascript
// NameSys configuration
const namesys = new NameSys({
  domain: 'mydomain.eth',
  gateway: 'https://gateway.namesys.xyz'
});

// Set up dynamic subdomains
await namesys.configureSubdomain({
  pattern: 'user-*',
  template: 'https://app.mydomain.eth/user/{username}',
  resolver: 'custom-resolver-address'
});
```

#### 3. NameSpace

NameSpace provides namespace-based subdomain management:

```javascript
// NameSpace setup
const namespace = new NameSpace({
  domain: 'mydomain.eth',
  namespace: 'app'
});

// Configure namespace rules
await namespace.setRule({
  pattern: '*.app.mydomain.eth',
  action: 'redirect',
  target: 'https://app.mydomain.eth/{subdomain}'
});
```

## Advanced Subdomain Patterns

### 1. User-Specific Subdomains

```javascript
// Pattern: user123.mydomain.eth
const userResolver = {
  resolve: (name) => {
    const username = extractUsername(name);
    return `ipfs://${getUserContent(username)}`;
  }
};
```

### 2. Environment-Based Subdomains

```javascript
// Pattern: dev.mydomain.eth, staging.mydomain.eth, prod.mydomain.eth
const environmentResolver = {
  resolve: (name) => {
    const env = extractEnvironment(name);
    return `ipfs://${getEnvironmentContent(env)}`;
  }
};
```

### 3. Geographic Subdomains

```javascript
// Pattern: us.mydomain.eth, eu.mydomain.eth, asia.mydomain.eth
const geoResolver = {
  resolve: (name) => {
    const region = extractRegion(name);
    return `ipfs://${getRegionalContent(region)}`;
  }
};
```

## Implementing CCIP with IPFS

### CCIP-Read with IPFS Gateway

```javascript
// CCIP resolver that fetches from IPFS
class IPFSCCIPResolver {
  async resolve(name, data) {
    const subdomain = this.extractSubdomain(name);
    
    // Fetch configuration from IPFS
    const configCID = await this.getConfigCID(subdomain);
    const config = await this.fetchFromIPFS(configCID);
    
    return {
      result: config.contentHash,
      validUntil: Date.now() + (config.ttl * 1000),
      resolver: this.address
    };
  }
  
  async fetchFromIPFS(cid) {
    const response = await fetch(`https://ipfs.io/ipfs/${cid}`);
    return await response.json();
  }
}
```

### Dynamic Content Updates

```javascript
// Update content without on-chain transactions
const updateContent = async (subdomain, newContent) => {
  // Upload new content to IPFS
  const newCID = await uploadToIPFS(newContent);
  
  // Update configuration (stored off-chain or in IPFS)
  await updateConfig(subdomain, {
    contentHash: `ipfs://${newCID}`,
    updatedAt: Date.now()
  });
};
```

## Best Practices

### 1. Security Considerations

- **Access Control**: Implement proper access controls for subdomain management
- **Rate Limiting**: Prevent abuse of wildcard resolvers
- **Validation**: Validate all inputs and subdomain names

### 2. Performance Optimization

- **Caching**: Implement caching for frequently accessed subdomains
- **CDN Integration**: Use CDNs for global content delivery
- **Lazy Loading**: Load subdomain configurations on-demand

### 3. Monitoring and Analytics

```javascript
// Track subdomain usage
const trackSubdomainAccess = (subdomain) => {
  analytics.track('subdomain_access', {
    subdomain,
    timestamp: Date.now(),
    userAgent: navigator.userAgent
  });
};
```

## Troubleshooting

### Common Issues

**Subdomain not resolving:**
- Check resolver contract configuration
- Verify CCIP implementation
- Ensure proper gas limits for resolution

**Wildcard not working:**
- Verify wildcard pattern matching
- Check resolver contract logic
- Ensure proper fallback handling

**Cross-chain resolution failing:**
- Verify CCIP implementation
- Check network connectivity
- Ensure proper error handling

## Tools and Resources

### Development Tools

- [ENS Manager](https://app.ens.domains/) - Official ENS management interface
- [NameStone](https://namestone.xyz/) - Wildcard resolver service
- [NameSys](https://namesys.xyz/) - Advanced subdomain management
- [NameSpace](https://namespace.xyz/) - Namespace-based management

### Documentation

- [ENS Documentation](https://docs.ens.domains/)
- [CCIP-Read Specification](https://eips.ethereum.org/EIPS/eip-3668)
- [IPFS Documentation](https://docs.ipfs.io/)

## Next Steps

With subdomains and CCIP mastered, explore:

- [Registry vs Resolver](registry-vs-resolver.md) - Deep dive into ENS architecture
- [Record Types](record-types.md) - Understanding different ENS record types
- [Alternatives to IPFS](alternatives-to-ipfs.md) - Other storage solutions
