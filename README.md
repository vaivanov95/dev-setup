# Dev setup

## Partitioning structure
Disk
* 100 MB - EFI System
* 930.29 GB - Windows (C:) NTFS
* 50 GB - RAW (for Ubuntu root)
* 8 GB - RAW (for Swap)
* 100 GB - NTFS (Shared)
* 800 GB - RAW (for K8s data)
* 781 MB - Recovery

## Security architecture
Core Idea:
* A memorized high-entropy passphrase is used as a root of trust
* Hardware key is used as a convenience tool, not a critical security component
* Hardware key is used for Web Authn always with other recovery methods (e.g. SMS)

### Daily Use
Data --[encrypted]--> Cloud Storage
            ^
            | master key
Hardware Key (convenient but not critical)

### Recovery
Data --[encrypted]--> Cloud Storage
            ^
            | master key
Encrypted Key Shares (stored publicly)
            ^
            | decryption key derived from
Memorized High-Entropy Passphrase
























