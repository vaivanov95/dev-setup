# Dev setup

2 systems are used: Windows 11 and Ubnntu.
When Windows is used, applications are run in a WSL Ubuntu instances, in a Kubernetes cluster. WSLg is used for GUI apps.
When Ubuntu is used, applications are run in a Kubernetes cluser like in Windows + WSL + Ubnutu case.
Both kubernetes clusters share persistant data.


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



## Plan


1. install Ubuntu using:
            50GB partition -> ext4, mount as /
            8GB partition -> swap

2. Set up LUKS encryption on the existing 100GB NTFS partition (this will preserve NTFS but add encryption)
3. Create LUKS+ext4 on the 800GB partition for K8s


The 100GB NTFS partition needs special handling because:

It's already formatted as NTFS
We want to keep NTFS for Windows compatibility
We need to encrypt it in a way both Windows and Ubuntu can use



















