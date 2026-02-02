# Running Your Own IPFS Node and IPNS Publishing

This guide covers how to run your own IPFS node for complete control over your dWebsite hosting and IPNS publishing, including republishing strategies and TTL management.

## ⚠️ Version Compatibility Note

**Important:** This guide provides general workflows and concepts. Specific commands and configuration options may vary based on your IPFS version. Always verify commands with your current installation:

```bash
ipfs --version
```

For the most up-to-date command reference, visit: https://docs.ipfs.io/reference/cli/

## Why Run Your Own IPFS Node?

Running your own IPFS node provides several advantages:

- **Complete Control**: You have full control over your data and don't rely on third-party pinning services
- **Privacy**: Your content isn't stored on external services
- **Cost-Effective**: No ongoing fees for pinning services
- **Customization**: You can configure your node exactly as needed
- **Reliability**: No dependency on external service availability

## Prerequisites

Before setting up your own IPFS node, ensure you have:

- A machine with at least 4GB RAM and 50GB storage
- Stable internet connection
- Basic command-line knowledge
- IPFS Kubo installed (see [How to Install IPFS Locally](../beginner/how-to-install-ipfs-locally/README.md))

## Setting Up Your IPFS Node

### 1. Initialize Your Node

```bash
# Initialize a new IPFS repository
ipfs init

# Start the IPFS daemon
ipfs daemon
```

### 2. Configure Your Node

Edit your IPFS configuration file located at `~/.ipfs/config`:

```json
{
  "Addresses": {
    "Swarm": [
      "/ip4/0.0.0.0/tcp/4001",
      "/ip6/::/tcp/4001"
    ],
    "API": "/ip4/127.0.0.1/tcp/5001",
    "Gateway": "/ip4/127.0.0.1/tcp/8080"
  },
  "Datastore": {
    "StorageMax": "10GB"
  },
  "Reprovider": {
    "Interval": "12h"
  }
}
```

### 3. Enable IPNS Publishing

Ensure IPNS is enabled in your configuration:

```bash
# Check if IPNS is enabled
ipfs config show | grep ipns

# If not enabled, add it
ipfs config --json API.HTTPHeaders.Access-Control-Allow-Origin '["*"]'
ipfs config --json API.HTTPHeaders.Access-Control-Allow-Methods '["PUT", "POST", "GET"]'
```

## IPNS Publishing and Republishing

### Understanding IPNS Keys

IPNS (InterPlanetary Name System) allows you to create mutable pointers to your content:

```bash
# Generate a new IPNS key
ipfs key gen my-website

# List your keys
ipfs key list -l

# Note: Verify command syntax with your IPFS version
# ipfs --version
```

### Publishing Content to IPNS

```bash
# Add your website files to IPFS
ipfs add -r /path/to/your/website

# Publish the CID to IPNS
ipfs name publish --key=my-website /ipfs/QmYourWebsiteCID
```

### Republishing Strategies

IPNS records have a Time-To-Live (TTL) and need to be republished periodically:

#### Manual Republishing

```bash
# Republish your IPNS record
ipfs name publish --key=my-website /ipfs/QmYourWebsiteCID
```

#### Automated Republishing

Create a script for automatic republishing:

```bash
#!/bin/bash
# republish.sh

WEBSITE_PATH="/path/to/your/website"
KEY_NAME="my-website"

# Add website files
CID=$(ipfs add -r -Q $WEBSITE_PATH)

# Publish to IPNS
ipfs name publish --key=$KEY_NAME $CID

echo "Published $CID to IPNS at $(date)"
```

Set up a cron job to run this script:

```bash
# Add to crontab (republish every 12 hours)
0 */12 * * * /path/to/republish.sh
```

### TTL Management

IPNS records have configurable TTL values:

```bash
# Publish with custom TTL (24 hours)
ipfs name publish --key=my-website --ttl=24h /ipfs/QmYourWebsiteCID

# Publish with custom TTL (7 days)
ipfs name publish --key=my-website --ttl=7d /ipfs/QmYourWebsiteCID
```

**Recommended TTL Values:**
- **12 hours**: For frequently updated content
- **24 hours**: For moderately updated content  
- **7 days**: For stable content
- **30 days**: For rarely updated content

> **Note:** TTL values may vary based on IPFS version and network conditions. Test with your specific setup.

## Advanced Configuration

### 1. Enable DHT Server Mode

For better network participation:

```bash
# Enable DHT server mode
ipfs config Routing.Type "dhtserver"
```

### 2. Configure Storage

Optimize your storage configuration:

```json
{
  "Datastore": {
    "StorageMax": "50GB",
    "StorageGCWatermark": 90,
    "GCPeriod": "1h"
  }
}
```

### 3. Set Up Monitoring

Monitor your node's health:

```bash
# Check node status
ipfs stats repo

# Check connected peers
ipfs swarm peers

# Check bandwidth usage
ipfs stats bw
```

## Connecting Your ENS Domain

Once your IPFS node is running and you've published to IPNS:

1. **Get your IPNS key hash**:
   ```bash
   ipfs key list -l
   ```

2. **Update your ENS content hash** with the IPNS key hash (prefixed with `/ipns/`)

3. **Access your dWebsite** through your ENS domain

## Troubleshooting

### Common Issues

**Node not starting:**
```bash
# Check if another IPFS process is running
ps aux | grep ipfs

# Kill existing process if needed
pkill ipfs
```

**IPNS not resolving:**
```bash
# Check IPNS record
ipfs name resolve /ipns/QmYourKeyHash

# Republish if needed
ipfs name publish --key=my-website /ipfs/QmYourWebsiteCID
```

**Storage issues:**
```bash
# Run garbage collection
ipfs repo gc

# Check storage usage
ipfs stats repo
```

### Performance Optimization

1. **Increase file descriptor limits**:
   ```bash
   ulimit -n 4096
   ```

2. **Use SSD storage** for better performance

3. **Configure appropriate memory limits** in your system

## Security Considerations

- **Firewall Configuration**: Only expose necessary ports
- **API Access**: Restrict API access to localhost
- **Key Management**: Secure your IPNS keys
- **Regular Updates**: Keep IPFS Kubo updated

## Next Steps

With your own IPFS node running, you can:

- [Configure your ENS domain](../beginner/configuring-your-ens-name/README.md)
- [Set up automated publishing workflows](how-to-publish-to-ipns.md)
- [Explore advanced IPFS features](../advanced/)

## Resources

- [IPFS Documentation](https://docs.ipfs.io/)
- [IPNS Specification](https://specs.ipfs.tech/ipns/ipns-record/)
- [IPFS Configuration Reference](https://github.com/ipfs/kubo/blob/master/docs/config.md)
