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



## Initial setup
1. Clone this repo to e.g. `~/dev-setup`
2. Mount shared NTFS partition to `/mnt/shared`
3. In `~/.bashrc` add at the beginning:
```bash

# Setup scripts and docs repo
DEV_SETUP_PATH=~/dev-setup
# Shared NTFS volume mount path
SHARED_PATH=/mnt/shared

source ${DEV_SETUP_PATH}/scripts/.init

```
** Note: in WSL instance shared path will be different **

### On system startup
Using local auth store, get encryption password and use it to mount the NTFS and ext4 (data) partitions.
After NTFS share is mounted switch to shared auth and update local.



## Plan

1. Set up LUKS encryption on the existing 100GB NTFS partition (this will preserve NTFS but add encryption)
2. Create LUKS+ext4 on the 800GB partition for K8s


The 100GB NTFS partition needs special handling because:

It's already formatted as NTFS
We want to keep NTFS for Windows compatibility
We need to encrypt it in a way both Windows and Ubuntu can use



















