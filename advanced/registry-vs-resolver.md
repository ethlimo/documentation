# Registry vs Resolver: Understanding ENS Architecture

This guide explains the fundamental components of the Ethereum Name Service (ENS) architecture: the Registry and Resolver contracts, and how they work together to provide decentralized naming.

## ENS Architecture Overview

ENS consists of two main smart contract components:

1. **Registry**: The core contract that manages domain ownership and subdomain delegation
2. **Resolver**: Contracts that translate names into addresses and other data

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   ENS Registry  │───▶│   Resolver      │───▶│   Target Data   │
│                 │    │                 │    │                 │
│ - Domain Owner  │    │ - ETH Address   │    │ - IPFS Hash     │
│ - Resolver      │    │ - IPFS Hash     │    │ - Website URL   │
│ - TTL           │    │ - TXT Records   │    │ - Email         │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

## The ENS Registry

The Registry is the core contract that maintains the mapping between domain names and their owners, resolvers, and TTL values.

### Registry Functions

```solidity
interface ENSRegistry {
    // Core domain management
    function setOwner(bytes32 node, address owner) external;
    function setResolver(bytes32 node, address resolver) external;
    function setTTL(bytes32 node, uint64 ttl) external;
    
    // Subdomain management
    function setSubnodeOwner(bytes32 node, bytes32 label, address owner) external;
    function setSubnodeRecord(bytes32 node, bytes32 label, address owner, address resolver, uint64 ttl) external;
    
    // Query functions
    function owner(bytes32 node) external view returns (address);
    function resolver(bytes32 node) external view returns (address);
    function ttl(bytes32 node) external view returns (uint64);
}
```

### Registry Data Structure

```solidity
struct Record {
    address owner;      // Domain owner
    address resolver;   // Resolver contract address
    uint64 ttl;        // Time-to-live for cached records
}
```

### Registry Operations

#### 1. Domain Registration

```javascript
// Register a new domain
const ensRegistry = new ethers.Contract(ENS_REGISTRY_ADDRESS, ENS_ABI, signer);

await ensRegistry.setSubnodeRecord(
    ethers.constants.HashZero, // Root node
    ethers.utils.keccak256(ethers.utils.toUtf8Bytes('mydomain')),
    ownerAddress,
    resolverAddress,
    0 // TTL
);
```

#### 2. Setting a Resolver

```javascript
// Set resolver for a domain
await ensRegistry.setResolver(
    ethers.utils.namehash('mydomain.eth'),
    resolverAddress
);
```

#### 3. Transferring Ownership

```javascript
// Transfer domain ownership
await ensRegistry.setOwner(
    ethers.utils.namehash('mydomain.eth'),
    newOwnerAddress
);
```

## The Resolver

Resolvers are contracts that implement the actual resolution logic, translating domain names into addresses, content hashes, and other data.

### Standard Resolver Interface

```solidity
interface IResolver {
    // Address resolution
    function addr(bytes32 node) external view returns (address);
    function setAddr(bytes32 node, address addr) external;
    
    // Content hash resolution
    function contenthash(bytes32 node) external view returns (bytes memory);
    function setContenthash(bytes32 node, bytes calldata hash) external;
    
    // Text record resolution
    function text(bytes32 node, string calldata key) external view returns (string memory);
    function setText(bytes32 node, string calldata key, string calldata value) external;
}
```

### Public Resolver Implementation

The Public Resolver is the most commonly used resolver implementation:

```solidity
contract PublicResolver {
    ENS public ens;
    
    // Address records
    mapping(bytes32 => address) private _addresses;
    
    // Content hash records
    mapping(bytes32 => bytes) private _contenthashes;
    
    // Text records
    mapping(bytes32 => mapping(string => string)) private _texts;
    
    function addr(bytes32 node) public view returns (address) {
        return _addresses[node];
    }
    
    function setAddr(bytes32 node, address addr) public {
        require(ens.owner(node) == msg.sender, "Not authorized");
        _addresses[node] = addr;
        emit AddrChanged(node, addr);
    }
    
    function contenthash(bytes32 node) public view returns (bytes memory) {
        return _contenthashes[node];
    }
    
    function setContenthash(bytes32 node, bytes calldata hash) public {
        require(ens.owner(node) == msg.sender, "Not authorized");
        _contenthashes[node] = hash;
        emit ContenthashChanged(node, hash);
    }
}
```

## Registry vs Resolver: Key Differences

| Aspect | Registry | Resolver |
|--------|----------|----------|
| **Purpose** | Domain ownership and delegation | Data resolution and storage |
| **Data Stored** | Owner, resolver address, TTL | Addresses, content hashes, text records |
| **Update Frequency** | Rare (ownership changes) | Frequent (content updates) |
| **Gas Costs** | High (ownership operations) | Low (data updates) |
| **Authorization** | Domain owner only | Domain owner or authorized accounts |

## How They Work Together

### Resolution Process

1. **Name to Node**: Convert domain name to node hash
2. **Registry Lookup**: Query registry for resolver address
3. **Resolver Query**: Query resolver for specific data
4. **Return Data**: Return resolved data to caller

```javascript
// Complete resolution process
async function resolveENS(domainName, recordType) {
    const node = ethers.utils.namehash(domainName);
    
    // Step 1: Get resolver from registry
    const resolverAddress = await ensRegistry.resolver(node);
    
    if (resolverAddress === ethers.constants.AddressZero) {
        throw new Error('No resolver set');
    }
    
    // Step 2: Query resolver for data
    const resolver = new ethers.Contract(resolverAddress, RESOLVER_ABI, provider);
    
    switch (recordType) {
        case 'addr':
            return await resolver.addr(node);
        case 'contenthash':
            return await resolver.contenthash(node);
        case 'text':
            return await resolver.text(node, 'url');
        default:
            throw new Error('Unknown record type');
    }
}
```

### Example: Setting Up a dWebsite

```javascript
// Current Public Resolver addresses (verify with ENS documentation)
const PUBLIC_RESOLVER_ADDRESSES = {
  mainnet: '0x4976fb03C32e5B8cfe2b6cCB31c09Ba78EBaBa41',
  goerli: '0x4B1488B7a6B320d2D721406204aBc3eeAa9AD329',
  sepolia: '0x8FADE66B79cC9f707aB26799354482EB93a5B7dD'
};

// 1. Set resolver (Registry operation)
await ensRegistry.setResolver(
    ethers.utils.namehash('mydomain.eth'),
    PUBLIC_RESOLVER_ADDRESSES.mainnet // Use appropriate network
);

// 2. Set content hash (Resolver operation)
const resolver = new ethers.Contract(PUBLIC_RESOLVER_ADDRESSES.mainnet, RESOLVER_ABI, signer);
await resolver.setContenthash(
    ethers.utils.namehash('mydomain.eth'),
    ethers.utils.toUtf8Bytes('ipfs://QmYourWebsiteHash')
);
```

## Advanced Resolver Patterns

### 1. Custom Resolvers

```solidity
contract CustomResolver {
    ENS public ens;
    
    // Custom resolution logic
    function resolve(bytes32 node, string calldata name) 
        external view returns (bytes memory) {
        
        // Implement custom resolution logic
        if (keccak256(abi.encodePacked(name)) == keccak256(abi.encodePacked("blog"))) {
            return abi.encode("ipfs://QmBlogContent");
        }
        
        return abi.encode("ipfs://QmDefaultContent");
    }
}
```

### 2. Multi-Signature Resolvers

```solidity
contract MultiSigResolver {
    mapping(bytes32 => mapping(address => bool)) public authorized;
    mapping(bytes32 => uint256) public requiredSignatures;
    
    modifier onlyAuthorized(bytes32 node) {
        require(authorized[node][msg.sender], "Not authorized");
        _;
    }
    
    function setContenthash(bytes32 node, bytes calldata hash) 
        external onlyAuthorized(node) {
        // Implementation with multi-sig logic
    }
}
```

### 3. Time-Based Resolvers

```solidity
contract TimeBasedResolver {
    mapping(bytes32 => mapping(uint256 => bytes)) public timeBasedContent;
    
    function getContentAtTime(bytes32 node, uint256 timestamp) 
        external view returns (bytes memory) {
        return timeBasedContent[node][timestamp];
    }
}
```

## Gas Optimization Strategies

### 1. Batch Operations

```javascript
// Batch multiple resolver operations
const batchResolver = new ethers.Contract(resolverAddress, BATCH_RESOLVER_ABI, signer);

await batchResolver.setMultipleRecords(
    node,
    ['addr', 'contenthash', 'text'],
    [address, contentHash, textValue]
);
```

### 2. Proxy Resolvers

```solidity
contract ProxyResolver {
    address public implementation;
    
    function setImplementation(address _implementation) external {
        implementation = _implementation;
    }
    
    fallback() external {
        // Delegate calls to implementation
        (bool success,) = implementation.delegatecall(msg.data);
        require(success, "Delegate call failed");
    }
}
```

## Security Considerations

### Registry Security

- **Ownership Management**: Secure domain ownership transfer
- **Access Control**: Restrict registry operations to authorized accounts
- **Emergency Procedures**: Implement emergency pause functionality

### Resolver Security

- **Authorization**: Proper access control for resolver updates
- **Input Validation**: Validate all inputs to prevent attacks
- **Upgradeability**: Consider upgradeable resolver patterns

## Monitoring and Analytics

### Registry Events

```solidity
event NewOwner(bytes32 indexed node, bytes32 indexed label, address owner);
event Transfer(bytes32 indexed node, address owner);
event NewResolver(bytes32 indexed node, address resolver);
```

### Resolver Events

```solidity
event AddrChanged(bytes32 indexed node, address addr);
event ContenthashChanged(bytes32 indexed node, bytes hash);
event TextChanged(bytes32 indexed node, string indexed indexedKey, string key);
```

## Best Practices

### 1. Registry Management

- Use a dedicated wallet for registry operations
- Implement proper backup and recovery procedures
- Monitor for unauthorized ownership changes

### 2. Resolver Configuration

- Choose appropriate resolver based on use case
- Implement proper access controls
- Consider gas costs for frequent updates

### 3. Testing

```javascript
// Test resolution workflow
describe('ENS Resolution', () => {
    it('should resolve domain correctly', async () => {
        const result = await resolveENS('mydomain.eth', 'contenthash');
        expect(result).to.equal('ipfs://QmExpectedHash');
    });
});
```

## Tools and Resources

### Development Tools

- [ENS Manager](https://app.ens.domains/) - Official ENS interface
- [Etherscan](https://etherscan.io/) - Contract verification and interaction
- [Hardhat](https://hardhat.org/) - Development framework

### Documentation

- [ENS Documentation](https://docs.ens.domains/)
- [ENS Contracts](https://github.com/ensdomains/ens-contracts)
- [EIP-137](https://eips.ethereum.org/EIPS/eip-137) - ENS Specification

## Next Steps

With a solid understanding of Registry vs Resolver, explore:

- [Record Types](record-types.md) - Deep dive into different ENS record types
- [ENS Subdomains and CCIP](ens-subdomains-ccip.md) - Advanced subdomain management
- [Alternatives to IPFS](alternatives-to-ipfs.md) - Other storage solutions
