# Arweave and ArNS: Permanent Storage and Decentralized Naming

This guide covers Arweave, a permanent decentralized storage solution, and ArNS (Arweave Name System), a decentralized naming system built on Arweave. Together, they provide a complete solution for permanent dWebsites and applications.

## What is Arweave?

Arweave is a decentralized storage network that enables permanent, low-cost data storage. Unlike traditional storage solutions that require ongoing payments, Arweave uses an endowment model where a one-time payment covers indefinite storage.

### Key Features

- **Permanent Storage**: Data is stored forever with a single payment
- **Proof of Access**: Novel consensus mechanism ensures data availability
- **Endowment Model**: One-time payment covers indefinite storage
- **Fast Retrieval**: Optimized for quick content access
- **Built-in Incentives**: Miners are rewarded for storing and serving data

## Getting Started with Arweave

### 1. Setting Up Your Environment

```bash
# Install Arweave JavaScript SDK
npm install arweave

# Install Arweave CLI (optional)
npm install -g arweave-cli
```

### 2. Creating a Wallet

```javascript
import Arweave from 'arweave';

// Initialize Arweave
const arweave = Arweave.init({
    host: 'arweave.net',
    port: 443,
    protocol: 'https'
});

// Note: Verify Arweave SDK version and API compatibility
// npm install arweave@latest

// Generate a new wallet
async function createWallet() {
    const wallet = await arweave.wallets.generate();
    const address = await arweave.wallets.jwkToAddress(wallet);
    
    console.log('Wallet address:', address);
    console.log('Wallet (keep secure):', JSON.stringify(wallet));
    
    return { wallet, address };
}

// Load existing wallet
async function loadWallet(walletPath) {
    const walletData = await fs.readFile(walletPath, 'utf8');
    const wallet = JSON.parse(walletData);
    const address = await arweave.wallets.jwkToAddress(wallet);
    
    return { wallet, address };
}
```

### 3. Funding Your Wallet

```javascript
// Check wallet balance
async function checkBalance(address) {
    const balance = await arweave.wallets.getBalance(address);
    const arBalance = arweave.ar.winstonToAr(balance);
    
    console.log(`Balance: ${arBalance} AR`);
    return arBalance;
}

// Get wallet balance in Winston (smallest unit)
async function getBalanceInWinston(address) {
    return await arweave.wallets.getBalance(address);
}
```

### 4. Uploading Content

```javascript
// Upload a single file
async function uploadFile(filePath, wallet, tags = {}) {
    const fileData = await fs.readFile(filePath);
    
    const transaction = await arweave.createTransaction({
        data: fileData
    }, wallet);
    
    // Add standard tags
    transaction.addTag('Content-Type', 'text/html');
    transaction.addTag('App-Name', 'dWebsite');
    
    // Add custom tags
    Object.entries(tags).forEach(([key, value]) => {
        transaction.addTag(key, value);
    });
    
    await arweave.transactions.sign(transaction);
    const response = await arweave.transactions.post(transaction);
    
    return transaction.id;
}

// Upload website directory
async function uploadWebsite(websitePath, wallet) {
    const files = await fs.readdir(websitePath);
    const uploadPromises = files.map(async (file) => {
        const filePath = path.join(websitePath, file);
        const stats = await fs.stat(filePath);
        
        if (stats.isFile()) {
            const contentType = getContentType(file);
            return await uploadFile(filePath, wallet, {
                'Content-Type': contentType,
                'File-Name': file
            });
        }
    });
    
    const transactionIds = await Promise.all(uploadPromises);
    return transactionIds.filter(id => id); // Remove undefined values
}

// Helper function to determine content type
function getContentType(filename) {
    const ext = path.extname(filename).toLowerCase();
    const contentTypes = {
        '.html': 'text/html',
        '.css': 'text/css',
        '.js': 'application/javascript',
        '.json': 'application/json',
        '.png': 'image/png',
        '.jpg': 'image/jpeg',
        '.jpeg': 'image/jpeg',
        '.gif': 'image/gif',
        '.svg': 'image/svg+xml'
    };
    
    return contentTypes[ext] || 'application/octet-stream';
}
```

### 5. Retrieving Content

```javascript
// Download content by transaction ID
async function downloadContent(transactionId) {
    const data = await arweave.transactions.getData(transactionId, {
        decode: true
    });
    
    return data;
}

// Get transaction metadata
async function getTransactionInfo(transactionId) {
    const transaction = await arweave.transactions.get(transactionId);
    
    return {
        id: transaction.id,
        owner: transaction.owner,
        tags: transaction.tags,
        data_size: transaction.data_size,
        block: transaction.block
    };
}

// Search for content by tags
async function searchByTags(tags) {
    const query = {
        op: 'and',
        expr1: {
            op: 'equals',
            expr1: 'App-Name',
            expr2: 'dWebsite'
        },
        expr2: {
            op: 'equals',
            expr1: 'Content-Type',
            expr2: 'text/html'
        }
    };
    
    const results = await arweave.arql(query);
    return results;
}
```

## What is ArNS?

ArNS (Arweave Name System) is a decentralized naming system built on Arweave that provides human-readable names for Arweave content. It's similar to ENS but specifically designed for Arweave's permanent storage model.

### Key Features

- **Human-Readable Names**: Map names to Arweave transaction IDs
- **Permanent Records**: Names are stored permanently on Arweave
- **Decentralized**: No central authority controls the naming system
- **Cost-Effective**: One-time payment for permanent name registration

## Setting Up ArNS

### 1. Understanding ArNS Structure

ArNS uses a hierarchical naming system:
- **Root Domain**: `.ar` (Arweave's equivalent of `.eth`)
- **Subdomains**: `mydomain.ar`, `blog.mydomain.ar`
- **Records**: Point to Arweave transaction IDs

### 2. Creating an ArNS Record

```javascript
// Create an ArNS record
async function createArNSRecord(name, targetTransactionId, wallet) {
    const record = {
        name: name,
        target: targetTransactionId,
        timestamp: Date.now(),
        version: '1.0'
    };
    
    const transaction = await arweave.createTransaction({
        data: JSON.stringify(record)
    }, wallet);
    
    // Add ArNS-specific tags
    transaction.addTag('App-Name', 'ArNS');
    transaction.addTag('App-Version', '1.0');
    transaction.addTag('Content-Type', 'application/json');
    transaction.addTag('Name', name);
    transaction.addTag('Target', targetTransactionId);
    
    await arweave.transactions.sign(transaction);
    await arweave.transactions.post(transaction);
    
    return transaction.id;
}

// Example usage
const websiteTransactionId = await uploadFile('./website/index.html', wallet);
await createArNSRecord('mydomain.ar', websiteTransactionId, wallet);
```

### 3. Resolving ArNS Names

```javascript
// Resolve an ArNS name to its target
async function resolveArNSName(name) {
    const query = {
        op: 'and',
        expr1: {
            op: 'equals',
            expr1: 'App-Name',
            expr2: 'ArNS'
        },
        expr2: {
            op: 'equals',
            expr1: 'Name',
            expr2: name
        }
    };
    
    const results = await arweave.arql(query);
    
    if (results.length === 0) {
        throw new Error(`ArNS name not found: ${name}`);
    }
    
    // Get the latest record (most recent transaction)
    const latestTransactionId = results[results.length - 1];
    const recordData = await arweave.transactions.getData(latestTransactionId, {
        decode: true
    });
    
    const record = JSON.parse(recordData);
    return record.target;
}

// Example usage
const targetId = await resolveArNSName('mydomain.ar');
const content = await downloadContent(targetId);
```

## Building a dWebsite with Arweave and ArNS

### 1. Complete Website Upload

```javascript
class ArweaveWebsite {
    constructor(wallet) {
        this.arweave = Arweave.init({
            host: 'arweave.net',
            port: 443,
            protocol: 'https'
        });
        this.wallet = wallet;
    }
    
    async uploadWebsite(websitePath, domainName) {
        console.log('Uploading website files...');
        
        // Upload all files
        const files = await this.getAllFiles(websitePath);
        const uploadResults = {};
        
        for (const file of files) {
            const relativePath = path.relative(websitePath, file);
            const transactionId = await this.uploadFile(file, {
                'File-Path': relativePath,
                'Website-Domain': domainName
            });
            
            uploadResults[relativePath] = transactionId;
            console.log(`Uploaded ${relativePath}: ${transactionId}`);
        }
        
        // Create manifest file
        const manifest = {
            domain: domainName,
            files: uploadResults,
            timestamp: Date.now(),
            version: '1.0'
        };
        
        const manifestId = await this.uploadManifest(manifest);
        
        // Create ArNS record
        await this.createArNSRecord(domainName, manifestId);
        
        return {
            manifestId,
            files: uploadResults
        };
    }
    
    async uploadFile(filePath, tags = {}) {
        const fileData = await fs.readFile(filePath);
        const contentType = this.getContentType(filePath);
        
        const transaction = await this.arweave.createTransaction({
            data: fileData
        }, this.wallet);
        
        transaction.addTag('Content-Type', contentType);
        transaction.addTag('App-Name', 'dWebsite');
        
        Object.entries(tags).forEach(([key, value]) => {
            transaction.addTag(key, value);
        });
        
        await this.arweave.transactions.sign(transaction);
        await this.arweave.transactions.post(transaction);
        
        return transaction.id;
    }
    
    async uploadManifest(manifest) {
        const transaction = await this.arweave.createTransaction({
            data: JSON.stringify(manifest)
        }, this.wallet);
        
        transaction.addTag('Content-Type', 'application/json');
        transaction.addTag('App-Name', 'dWebsite-Manifest');
        
        await this.arweave.transactions.sign(transaction);
        await this.arweave.transactions.post(transaction);
        
        return transaction.id;
    }
    
    async createArNSRecord(name, targetId) {
        const record = {
            name: name,
            target: targetId,
            timestamp: Date.now()
        };
        
        const transaction = await this.arweave.createTransaction({
            data: JSON.stringify(record)
        }, this.wallet);
        
        transaction.addTag('App-Name', 'ArNS');
        transaction.addTag('Content-Type', 'application/json');
        transaction.addTag('Name', name);
        transaction.addTag('Target', targetId);
        
        await this.arweave.transactions.sign(transaction);
        await this.arweave.transactions.post(transaction);
        
        return transaction.id;
    }
    
    async getAllFiles(dirPath) {
        const files = [];
        
        const items = await fs.readdir(dirPath);
        for (const item of items) {
            const fullPath = path.join(dirPath, item);
            const stats = await fs.stat(fullPath);
            
            if (stats.isDirectory()) {
                files.push(...await this.getAllFiles(fullPath));
            } else {
                files.push(fullPath);
            }
        }
        
        return files;
    }
    
    getContentType(filePath) {
        const ext = path.extname(filePath).toLowerCase();
        const contentTypes = {
            '.html': 'text/html',
            '.css': 'text/css',
            '.js': 'application/javascript',
            '.json': 'application/json',
            '.png': 'image/png',
            '.jpg': 'image/jpeg',
            '.jpeg': 'image/jpeg',
            '.gif': 'image/gif',
            '.svg': 'image/svg+xml'
        };
        
        return contentTypes[ext] || 'application/octet-stream';
    }
}
```

### 2. Website Deployment

```javascript
// Deploy a complete website
async function deployWebsite(websitePath, domainName, walletPath) {
    const walletData = await fs.readFile(walletPath, 'utf8');
    const wallet = JSON.parse(walletData);
    
    const arweaveWebsite = new ArweaveWebsite(wallet);
    
    console.log(`Deploying website to ${domainName}.ar...`);
    
    const result = await arweaveWebsite.uploadWebsite(websitePath, domainName);
    
    console.log('Deployment complete!');
    console.log(`Manifest: ${result.manifestId}`);
    console.log(`Access your site at: https://${domainName}.ar`);
    
    return result;
}

// Example usage
deployWebsite('./my-website', 'mydomain', './wallet.json');
```

## Integrating with ENS

### 1. Setting Arweave Contenthash in ENS

```javascript
// Set Arweave content hash in ENS
async function setArweaveInENS(ensDomain, arweaveTransactionId) {
    const resolver = new ethers.Contract(resolverAddress, RESOLVER_ABI, signer);
    const node = ethers.utils.namehash(ensDomain);
    
    // Encode Arweave transaction ID for contenthash
    const encoded = ethers.utils.hexlify(
        ethers.utils.concat([
            0x6b, // Arweave multicodec
            ethers.utils.toUtf8Bytes(arweaveTransactionId)
        ])
    );
    
    await resolver.setContenthash(node, encoded);
}

// Example: Set both ArNS and ENS
async function setDualNames(arweaveId, ensDomain) {
    // Set ArNS record
    await createArNSRecord('mydomain.ar', arweaveId, wallet);
    
    // Set ENS contenthash
    await setArweaveInENS(ensDomain, arweaveId);
    
    console.log(`Content accessible via:`);
    console.log(`- ArNS: https://mydomain.ar`);
    console.log(`- ENS: https://${ensDomain}`);
}
```

### 2. Multi-Protocol Resolution

```javascript
class MultiProtocolResolver {
    constructor(arweave, ensResolver) {
        this.arweave = arweave;
        this.ensResolver = ensResolver;
    }
    
    async resolveContent(domain) {
        // Try ArNS first
        try {
            const arweaveId = await this.resolveArNS(domain);
            return {
                protocol: 'arweave',
                id: arweaveId,
                url: `https://arweave.net/${arweaveId}`
            };
        } catch (error) {
            console.log('ArNS resolution failed, trying ENS...');
        }
        
        // Try ENS
        try {
            const contenthash = await this.ensResolver.contenthash(domain);
            const decoded = this.decodeContenthash(contenthash);
            
            if (decoded.system === 'arweave') {
                return {
                    protocol: 'arweave',
                    id: decoded.content,
                    url: `https://arweave.net/${decoded.content}`
                };
            }
        } catch (error) {
            console.log('ENS resolution failed');
        }
        
        throw new Error(`Could not resolve ${domain}`);
    }
    
    async resolveArNS(name) {
        // ArNS resolution logic (as shown above)
        return await resolveArNSName(name);
    }
    
    decodeContenthash(encoded) {
        // Contenthash decoding logic (as shown in Record Types guide)
        // Implementation here...
    }
}
```

## Cost Analysis

### Arweave Storage Costs

Arweave uses a one-time payment model based on:
- **Data size**: Larger files cost more
- **Network demand**: Prices fluctuate based on usage
- **Storage duration**: Endowment model covers indefinite storage

```javascript
// Calculate storage cost
async function calculateStorageCost(dataSize) {
    const price = await arweave.transactions.getPrice(dataSize);
    const costInAR = arweave.ar.winstonToAr(price);
    
    console.log(`Storage cost for ${dataSize} bytes: ${costInAR} AR`);
    return costInAR;
}

// Example: Calculate cost for a 1MB file
const oneMB = 1024 * 1024;
const cost = await calculateStorageCost(oneMB);
```

### Cost Comparison

| Storage Solution | Cost Model | 1GB Cost | Permanence |
|------------------|------------|----------|------------|
| Arweave | One-time | ~$0.50-1.00 | Permanent |
| IPFS + Pinning | Monthly | $5-20/month | Temporary |
| Traditional Cloud | Monthly | $0.02-0.10/month | Temporary |

## Best Practices

### 1. Content Organization

- **Use meaningful tags**: Tag your content for easy discovery
- **Create manifests**: Use manifest files to organize multiple files
- **Version your content**: Include version information in tags
- **Document your structure**: Maintain clear documentation of your content organization

### 2. Cost Optimization

- **Compress content**: Reduce file sizes before uploading
- **Bundle files**: Use Arweave's bundling features for multiple files
- **Optimize images**: Use appropriate image formats and sizes
- **Remove duplicates**: Avoid storing duplicate content

### 3. Security Considerations

- **Secure wallet storage**: Keep your Arweave wallet secure
- **Backup wallet**: Maintain secure backups of your wallet
- **Verify content**: Always verify uploaded content
- **Monitor costs**: Track storage costs and wallet balances

### 4. Performance Optimization

- **Use CDNs**: Consider using Arweave gateways for faster access
- **Optimize file structure**: Organize files for efficient retrieval
- **Cache frequently accessed content**: Implement caching strategies
- **Monitor performance**: Track content access times and optimize

## Tools and Resources

### Development Tools

- [Arweave JS](https://github.com/ArweaveTeam/arweave-js) - Official JavaScript SDK
- [Arweave CLI](https://github.com/ArweaveTeam/arweave-cli) - Command-line interface
- [Arweave Gateway](https://arweave.net/) - Web gateway for Arweave content

### ArNS Tools

- [ArNS SDK](https://github.com/ArweaveTeam/arns-js) - JavaScript SDK for ArNS
- [ArNS Gateway](https://arweave.net/) - Gateway supporting ArNS resolution

### Documentation

- [Arweave Documentation](https://docs.arweave.org/)
- [ArNS Specification](https://github.com/ArweaveTeam/arns)
- [Arweave Whitepaper](https://arweave.org/whitepaper.pdf)

## Next Steps

With Arweave and ArNS mastered, explore:

- [Alternatives to IPFS](alternatives-to-ipfs.md) - Other storage solutions
- [ENS Subdomains and CCIP](ens-subdomains-ccip.md) - Advanced ENS features
- [Record Types](record-types.md) - Understanding ENS record systems
