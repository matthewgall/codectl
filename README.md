# codectl

**VS Code Server Control** - A comprehensive version manager for VS Code servers


[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Shell Script](https://img.shields.io/badge/Shell-Bash-4EAA25.svg)](https://www.gnu.org/software/bash/)
[![Platform](https://img.shields.io/badge/Platform-Linux-blue.svg)](https://www.linux.org/)

> Modern version management for code-server and openvscode-server with GitHub integration, dual-mode operation, and comprehensive service management.

## ✨ Features

### 🎯 **Smart Version Management**
- **GitHub API Integration** - Discover and validate versions from official releases
- **Latest Version Detection** - Install newest stable releases automatically  
- **Symlink-based Switching** - Instant version switching without data loss
- **Intelligent Rollback** - Safe fallback to previous working versions

### 🔄 **Dual-Mode Operation**
- **User Mode** - No root required, installs to `~/.codectl/`
- **System Mode** - Root installation to `/opt/codectl/` with proper ownership
- **Automatic Detection** - Seamlessly adapts based on execution context

### 🛠️ **Multi-Server Support**
- **code-server** - Full support for Coder's VS Code server
- **openvscode** - Complete OpenVSCode Server integration
- **VSCodium** - Native VSCodium server support
- **Extensible Architecture** - Easy to add new VS Code server variants

### ⚙️ **Service Management**
- **Systemd Integration** - User or system service creation
- **Auto-restart** - Configures services with failure recovery
- **Service Preservation** - Maintains service state during upgrades

### 🔒 **Production Ready**
- **Atomic Operations** - Safe downloads with verification
- **Dry Run Support** - Preview changes before execution
- **Comprehensive Validation** - Version format and remote availability checks
- **Cleanup Management** - Automatic cleanup of old versions

## 🚀 Quick Start

### Installation
```bash
# Download codectl
wget https://raw.githubusercontent.com/your-repo/codectl/main/codectl
chmod +x codectl

# Move to system PATH (optional)
sudo mv codectl /usr/local/bin/
```

### Basic Usage
```bash
# Install latest code-server
codectl --latest

# Install specific version  
codectl 4.103.2

# Install with service creation
codectl --latest --install-service

# List available versions
codectl --list-remote
```

## 📖 Usage Guide

### Version Discovery
```bash
# List remote versions from GitHub
codectl --list-remote                    # code-server versions
codectl --list-remote --type openvscode  # openvscode versions
codectl --list-remote --type vscodium    # VSCodium versions

# Install latest stable version
codectl --latest                         # latest code-server
codectl --latest --type openvscode       # latest openvscode
codectl --latest --type vscodium         # latest VSCodium
```

### Version Management
```bash
# Install specific versions
codectl 4.103.2                         # code-server
codectl 1.103.1 --type openvscode       # openvscode
codectl 1.103.25610 --type vscodium     # VSCodium

# List installed versions
codectl --list

# Check current version
codectl --current

# Rollback to previous version
codectl --rollback

# Clean up old versions (keeps current + 2 previous)
codectl --cleanup
```

### Service Management
```bash
# Install with automatic service creation
codectl --latest --install-service

# Create service for current installation  
codectl --install-service

# Check service status
codectl --service-status

# Remove service
codectl --remove-service
```

### Advanced Operations
```bash
# Preview operations without changes
codectl --latest --dry-run
codectl 4.103.2 --install-service --dry-run

# System-wide installation (requires root)
sudo codectl --latest --install-service

# Custom user/group (root mode)
sudo CODECTL_USER=developer CODECTL_GROUP=developer codectl 4.103.2
```

## 🏗️ Architecture

### Directory Structure

#### User Mode (Default)
```
~/.codectl/
├── current → versions/code-server-4.103.2/    # Active version symlink
├── versions/                                   # All installed versions
│   ├── code-server-4.103.2/
│   ├── code-server-4.103.1/
│   ├── openvscode-1.103.1/
│   └── vscodium-1.103.25610/
└── tmp/                                       # Temporary operations
```

#### System Mode (Root)
```
/opt/codectl/
├── current → versions/code-server-4.103.2/    # Active version symlink
├── versions/                                   # All installed versions
│   ├── code-server-4.103.2/
│   ├── code-server-4.103.1/
│   ├── openvscode-1.103.1/
│   └── vscodium-1.103.25610/
└── tmp/                                       # Temporary operations
```

### Supported Servers

| Server | Repository | Default Port | Binary |
|--------|------------|--------------|--------|
| **code-server** | coder/code-server | 8080 | `code-server` |
| **openvscode** | gitpod-io/openvscode-server | 8080 | `openvscode-server` |
| **VSCodium** | VSCodium/vscodium | 8080 | `vscodium` |

## ⚙️ Configuration

### Environment Variables

#### User Mode
- `CODECTL_HOME` - Installation directory (default: `~/.codectl`)

#### System Mode (Root)
- `CODECTL_HOME` - Installation directory (default: `/opt/codectl`)
- `CODECTL_USER` - Target user for file ownership (default: `coder`)
- `CODECTL_GROUP` - Target group for file ownership (default: `coder`)

### Examples
```bash
# Custom installation directory
CODECTL_HOME=/custom/path codectl --latest

# Root mode with custom user
sudo CODECTL_USER=developer CODECTL_GROUP=staff codectl 4.103.2
```

## 🛡️ Safety Features

- **✅ Atomic Operations** - Uses temporary directories with full validation
- **✅ Service State Preservation** - Maintains service status during upgrades  
- **✅ Rollback Protection** - Safe fallback with backup symlinks
- **✅ Version Validation** - GitHub API verification of version availability
- **✅ File Verification** - Download integrity checks with format validation
- **✅ Permission Management** - Proper ownership in both user and system modes
- **✅ Dry Run Support** - Preview all operations before execution

## 📋 Requirements

- **Operating System**: Linux (systemd-based distributions)
- **Shell**: Bash 4.0+
- **Dependencies**: `wget`, `tar`, `systemctl` (usually pre-installed)
- **Permissions**: User mode (no special permissions) or root for system-wide installation

## 🤝 Contributing

Contributions are welcome! The codebase is designed for extensibility:

### Adding New Server Types
1. Update the `get_server_config()` function with new server details
2. Add validation in `validate_server_type()`
3. All other functionality automatically inherits support

### Architecture Principles
- **DRY (Don't Repeat Yourself)** - Centralized configuration system
- **Modularity** - Helper functions for common operations
- **Flexibility** - Template-based URLs and dynamic command usage
- **Safety First** - Comprehensive validation and error handling

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- **[Coder](https://github.com/coder/code-server)** - For the excellent code-server project
- **[Gitpod](https://github.com/gitpod-io/openvscode-server)** - For OpenVSCode Server
- **VS Code Community** - For making remote development accessible

---

**Made with ❤️ for the VS Code community**

*Like nvm for Node.js, but for VS Code servers*