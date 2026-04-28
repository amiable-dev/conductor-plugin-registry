# Conductor Plugin Registry

Official plugin marketplace for [Conductor](https://github.com/amiable-dev/conductor) - a MIDI controller mapping system with WASM plugin support.

## Overview

This repository hosts the official registry of verified WASM plugins for Conductor. Users can browse, download, and install plugins directly from the Conductor GUI or CLI.

## Registry Structure

The registry is a JSON file at `registry.json` containing:
- **Plugin metadata** (name, version, author, description)
- **Download URLs** for WASM binaries and signatures
- **Capabilities** required by each plugin
- **Categories and tags** for organization
- **Verification status** (signed, verified, trusted)

## Accessing the Registry

### Via Conductor GUI
1. Open Conductor GUI
2. Navigate to **Plugins** → **Marketplace**
3. Browse available plugins by category
4. Click **Install** to download and verify plugins

### Via Conductor CLI
```bash
# List all available plugins
conductorctl plugin list-available

# Install a plugin
conductorctl plugin install spotify

# Search for plugins
conductorctl plugin search "music"
```

### Direct URL
```
https://raw.githubusercontent.com/amiable-dev/conductor-plugin-registry/main/registry.json
```

## Available Plugins

### Music & Audio
- **Spotify Web API** - Control Spotify playback, search, and playlists
  - Play/pause, skip tracks, volume control
  - Search songs, artists, albums
  - Manage playlists and queue
  - Requires: Network capability

### Streaming & Video
- **OBS Studio Control** - Control OBS via WebSocket API
  - Switch scenes and sources
  - Control filters and streaming
  - Trigger recordings and virtual camera
  - Requires: Network capability

### System Utilities
- **System Utilities** - General-purpose system tools
  - Clipboard management (copy/paste)
  - System notifications
  - File operations
  - Process control
  - Requires: Subprocess, Filesystem, SystemControl capabilities

## Plugin Verification

Conductor uses a three-tier trust model:

### 1. Unsigned Plugins
- No cryptographic signature
- SHA-256 checksum verification only
- User must manually approve installation
- ⚠️ Use with caution

### 2. Self-Signed Plugins
- Signed with Ed25519 digital signature
- Verifies author identity and binary integrity
- Protects against tampering
- ✓ Safe if you trust the author

### 3. Trusted Plugins
- Signed by trusted public keys
- Official Conductor team or verified publishers
- Highest security level
- ✓✓ Recommended for production use

## Plugin Schema

Each plugin entry includes:

```json
{
  "id": "unique-plugin-id",
  "name": "Plugin Display Name",
  "version": "0.1.0",
  "author": "Author Name",
  "description": "Brief description of functionality",
  "category": "music|streaming|utilities|daw|gaming",
  "tags": ["tag1", "tag2"],
  "capabilities": ["Network", "Filesystem", "Subprocess"],
  "download_url": "https://github.com/.../plugin.wasm",
  "signature_url": "https://github.com/.../plugin.wasm.sig",
  "checksum": "sha256:...",
  "size_bytes": 524288,
  "license": "MIT",
  "repository": "https://github.com/...",
  "documentation": "https://...",
  "min_conductor_version": "2.5.0",
  "signed": false,
  "verified": false
}
```

## Contributing Plugins

### For Plugin Developers

To submit a plugin to the official registry:

1. **Build Your Plugin**
   ```bash
   cd plugins/your-plugin
   cargo build --release --target wasm32-wasip1
   ```

2. **Sign Your Plugin** (optional but recommended)
   ```bash
   # Generate Ed25519 key pair
   conductor-sign generate-keypair --output-dir ~/.conductor/keys

   # Sign the plugin
   conductor-sign sign \
     --input target/wasm32-wasip1/release/your_plugin.wasm \
     --private-key ~/.conductor/keys/private.key \
     --signer-name "Your Name" \
     --signer-email "you@example.com"
   ```

3. **Calculate SHA-256 Checksum**
   ```bash
   sha256sum target/wasm32-wasip1/release/your_plugin.wasm
   ```

4. **Create a GitHub Release**
   - Upload `your_plugin.wasm`
   - Upload `your_plugin.wasm.sig` (if signed)
   - Note the download URLs

5. **Submit Pull Request**
   - Add your plugin entry to `registry.json`
   - Include all required metadata fields
   - Provide documentation URL
   - Add tests verifying your plugin works

### Review Process

1. **Automated Checks**
   - JSON schema validation
   - Download URL accessibility
   - Checksum verification
   - WASM binary validation

2. **Manual Review**
   - Code quality and security audit
   - Capability usage justification
   - Documentation completeness
   - License compatibility

3. **Trust Tier Assignment**
   - New contributors: Self-signed or unsigned
   - Verified publishers: Request trusted key addition
   - Official plugins: Signed by Conductor team key

## Security Best Practices

### For Users
- ✓ Always verify plugin signatures before installation
- ✓ Review required capabilities carefully
- ✓ Check plugin author and repository
- ✓ Read documentation before granting permissions
- ⚠️ Avoid unsigned plugins from unknown sources

### For Developers
- ✓ Sign all production releases
- ✓ Request minimum required capabilities
- ✓ Document all network/filesystem access
- ✓ Keep dependencies up to date
- ✓ Provide clear privacy policy
- ⚠️ Never bundle credentials in WASM binary

## Plugin Categories

- **music** - Music streaming, audio control, DAW integration
- **streaming** - Video streaming, broadcast control, recording
- **utilities** - System utilities, productivity tools
- **daw** - Digital Audio Workstation control and integration
- **gaming** - Game control and integration plugins

## Capabilities Reference

Plugins declare required system capabilities:

- **Network** - HTTP/WebSocket API access
- **Filesystem** - File read/write (sandboxed directories)
- **Subprocess** - Execute shell commands
- **SystemControl** - Volume, notifications, clipboard
- **MidiOutput** - Send MIDI messages to external devices

## License

This registry is MIT licensed. Individual plugins may use different licenses - check each plugin's metadata.

## Support

- **Documentation**: https://code.claude.com/docs/en/plugins
- **Issues**: https://github.com/amiable-dev/conductor/issues
- **Discussions**: https://github.com/amiable-dev/conductor/discussions

---

**Registry Version**: 1.0.0  
**Last Updated**: 2025-11-19  
**Total Plugins**: 3
