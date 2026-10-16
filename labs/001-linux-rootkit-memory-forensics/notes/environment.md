# Lab Environment

> **Lab:** 001 — Linux Rootkit Detection & Memory Forensics  
> **Status:** IN PROGRESS  
> **Purpose:** Document the laboratory configuration and provide a reproducible record of the environment used during analysis.

---

## 1. Overview

This document records the virtual machines, operating systems, kernel versions, tools, network configuration, and symbol sources used in the Diamorphine memory-forensics experiment.

The laboratory uses two separate virtual machines:

- **Target VM:** Ubuntu Linux, used for the controlled experiment and memory acquisition.
- **Forensic VM:** REMnux, used for offline memory analysis with Volatility 3.

Record the actual configuration used during each experiment. Do not assume that the environment matches the proposed configuration until it has been verified.

## 2. Virtualization

| Property | Target VM | Forensic VM |
|---|---|---|
| Hypervisor | TBD | TBD |
| Operating system | Ubuntu Linux | REMnux |
| OS release | TBD | TBD |
| Architecture | TBD | TBD |
| Allocated memory | TBD | TBD |
| Virtual disk size | TBD | TBD |
| Network mode | Host-only / isolated | Host-only / isolated |
| Initial snapshot | `clean` | TBD |

### Network isolation

The target VM is isolated from production networks and the public Internet during the experiment.

Document the actual network configuration, including any permitted communication between the target and forensic workstation.

- [ ] Target VM is not using bridged networking.
- [ ] Internet access has been disabled or otherwise appropriately restricted.
- [ ] Shared folders and clipboard integration have been disabled.
- [ ] Host integrations unnecessary for the experiment have been disabled.
- [ ] A clean target snapshot has been created.
- [ ] File-transfer procedures between VMs have been documented.

Virtualization is a risk-reduction measure, not a guarantee of containment.

## 3. Target Operating System

Record the target configuration before introducing the research specimen.

| Property | Value |
|---|---|
| Distribution | TBD |
| Release | TBD |
| Kernel version (`uname -r`) | TBD |
| Kernel architecture | TBD |
| Kernel package | TBD |
| Kernel headers installed | TBD |
| Kernel debugging symbols available | TBD |
| Secure Boot state, if applicable | TBD |
| Kernel taint value before experiment | TBD |

Useful identification commands:

```bash
cat /etc/os-release
uname -a
uname -r
uname -m
cat /proc/sys/kernel/tainted
```

### Kernel compatibility

The kernel version is a critical part of the experiment. Kernel module compatibility and memory-forensics symbol compatibility are separate requirements.

Record the exact kernel release and any compatibility issues encountered. If the kernel changes, document the change and reassess the environment before comparing results.

## 4. Research Specimen

| Property | Value |
|---|---|
| Name | Diamorphine |
| Upstream repository | https://github.com/m0nad/Diamorphine |
| Acquisition method | Upstream Git repository |
| Git commit | TBD |
| Acquisition date | TBD |
| Build outcome | TBD |
| Target kernel | TBD |
| Compiled module SHA-256 | TBD, if applicable |

The source code is obtained from the upstream project rather than redistributed by this repository.

Record the precise commit used so that subsequent researchers can identify the corresponding source revision.

## 5. Forensic Workstation

| Property | Value |
|---|---|
| Distribution | REMnux |
| Release / image version | TBD |
| Architecture | TBD |
| Python version | TBD |
| Volatility 3 version | TBD |
| Volatility installation method | TBD |
| `dwarf2json` version | TBD |
| Available disk space | TBD |

Record the installed tool versions using the relevant version commands or package metadata.

The exact commands may differ depending on whether a tool was installed through a package manager, Python environment, or a compiled binary.

## 6. Memory Acquisition

| Property | Value |
|---|---|
| Acquisition tool | AVML |
| AVML version | TBD |
| Acquisition method | Guest-based / hypervisor-based |
| Acquisition date and time | TBD |
| Output filename | TBD |
| Output format | TBD |
| Image size | TBD |
| SHA-256 | TBD |
| Transfer method | TBD |
| Destination hash verified | TBD |

The memory image is generated locally and is not distributed with this repository.

Memory images may contain credentials, tokens, private keys, session data, and other sensitive information. Store the original image securely and publish only reviewed, sanitized evidence.

## 7. Kernel Symbols

| Property | Value |
|---|---|
| Target kernel banner | TBD |
| Symbol source | TBD |
| Debug information package / build | TBD |
| `dwarf2json` version | TBD |
| Generated ISF filename | TBD |
| ISF SHA-256 | TBD |
| Volatility symbol location | TBD |
| Symbol compatibility verified | TBD |

Document whether a pre-generated symbol table was used or an ISF was generated from matching kernel debugging information.

A matching distribution name alone does not establish symbol compatibility.

## 8. Baseline and Evidence

Record the location of the sanitized evidence files stored in the repository.

| Evidence | Location | Status |
|---|---|---|
| Initial system information | `evidence/baseline/` | Pending |
| Initial process enumeration | `evidence/baseline/` | Pending |
| Initial module enumeration | `evidence/baseline/` | Pending |
| Post-experiment observations | `evidence/post-experiment/` | Pending |
| Volatility 3 output | `evidence/volatility/` | Pending |
| Screenshots | `screenshots/` | Pending |
| Acquisition metadata | Local record / sanitized manifest | Pending |

Do not include the full memory image, credentials, or other sensitive data in the public evidence directories.

## 9. Reproducibility Notes

Use this section to document any changes from the intended configuration.

- Kernel updates:
- Package availability:
- Build or compatibility issues:
- Symbol-resolution errors:
- Acquisition warnings:
- Differences between runs:
- Other environmental constraints:

## 10. Change Log

| Date | Change | Reason |
|---|---|---|
| TBD | Initial environment record | Lab preparation |

---

*Part of `rem labs` — Learn by observing. Verify by reproducing.*