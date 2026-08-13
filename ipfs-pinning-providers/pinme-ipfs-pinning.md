# PinMe IPFS Pinning

Content uploaded with PinMe is validated, processed onto IPFS/Filecoin, and pinned automatically — the CLI returns the cryptographic hash (CID) of your content when the upload completes. Any file type is accepted: websites, images, audio, video, documents, and configuration files.

Make sure you have [installed the CLI and logged in](pinme.md) before uploading.

### Uploading Files and Directories

The `upload` command transfers a file or directory to IPFS:

```bash
pinme upload [path] [--domain <name>]
pinme upload [path] [-d <name>]
```

* **path** (optional) — file or directory to upload. If omitted, PinMe launches an interactive mode that prompts you to select a file or directory.
* **-d, --domain** (optional) — bind the upload to a PinMe subdomain or custom DNS domain.

Examples:

```bash
# Upload a single file
pinme upload ./image.png

# Upload a build output directory
pinme upload ./dist

# Upload documentation and bind it to a domain
pinme upload ./docs --domain my-docs-site
```

### Limits

| Constraint | Limit |
| ---------- | ----- |
| Single file | 200 MB |
| Directory (total) | 1 GB |
| Domain names | 3–63 characters; lowercase letters, numbers, and hyphens only; cannot start or end with a hyphen |

{% hint style="info" %}
Limits may change between releases — verify with `pinme upload --help` and the [PinMe upload documentation](https://pinme.eth.limo/upload-files.md) for your installed version.
{% endhint %}

Optional domain binding takes 1–2 minutes to propagate.

### Using Your CID with ENS

Once the upload completes, copy the returned CID and set it as the content hash of your ENS name (see [Updating Your ENS Content Records](../beginner/configuring-your-ens-name/updating-your-ens-content-records.md)). Your dWebsite is then reachable through any ENS gateway:

```
https://yourname.eth.limo
```

### Managing Uploads

```bash
pinme list                                 # Show upload history
pinme list -l 5                            # Show the 5 most recent uploads
pinme rm <value>                           # Remove content
pinme import ./site.car                    # Import a CAR file
pinme export <cid> --output ./exports      # Export content as a CAR file
```

### Best Practices

* Use relative paths in your HTML/CSS/JavaScript (e.g. `./styles/main.css`) rather than absolute paths, so the site works from any gateway or CID-based URL.
* Compress images and minify code before uploading.
* Upload built assets only (e.g. `dist/`, `build/`, `out/`, `public/`) — PinMe deploys static sites, not source code or dynamic backends.
