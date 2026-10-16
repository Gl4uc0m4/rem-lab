# Linux Rootkit Detection & Memory Forensics

> **Status:** ![In Progress](https://img.shields.io/badge/status-IN_PROGRESS-9AFF00?style=for-the-badge)
>
> **Focus:** Linux kernel internals · Rootkit detection · Memory forensics
>
> **Analysis stack:** Ubuntu Linux · REMnux · Volatility 3
>
> **Specimen:** [Diamorphine](https://github.com/m0nad/Diamorphine) — Linux kernel rootkit

---

## Overview

This lab explores Linux rootkit detection through memory forensics, using [Diamorphine](https://github.com/m0nad/Diamorphine) as a research specimen and [Volatility 3](https://github.com/volatilityfoundation/volatility3) as the primary memory-analysis framework.

The objective is to investigate discrepancies between information exposed by a running operating system and artifacts recovered from a memory image. Particular attention is given to process enumeration, kernel module visibility, kernel state, and the capabilities and limitations of memory-forensics plugins when examining a potentially compromised Linux system.

The investigation follows a reproducible workflow:

1. Prepare an isolated laboratory environment.
2. Record a clean baseline.
3. Conduct a controlled experiment using a disposable target VM.
4. Acquire a memory image.
5. identify the target kernel and prepare compatible symbols.
6. Analyze the image from a separate forensic workstation.
7. Compare observations and document the findings.

This lab is intended for education, defensive security research, and understanding Linux rootkit detection techniques.

**The memory image is generated locally during the experiment and is not distributed with this repository.**

## Objectives

- Understand Linux kernel modules and kernel-level rootkit behavior.
- Establish a baseline of process and module visibility.
- Investigate discrepancies between live-system enumeration and memory-forensics results.
- Acquire and preserve a memory image for offline analysis.
- Obtain or generate compatible Linux kernel symbols for Volatility 3.
- Evaluate the capabilities and limitations of selected Volatility 3 plugins.
- Document findings, false positives, environmental constraints, and reproducibility issues.
- Preserve sufficient metadata and evidence for other researchers to repeat the experiment.

## Lab Architecture

The laboratory uses two virtual machines to separate the experimental target from the forensic workstation.

| Component | Role |
|---|---|
| Ubuntu Linux VM | Isolated experimental target |
| REMnux VM | Forensic workstation |
| Diamorphine | Linux kernel rootkit research specimen |
| AVML | Linux memory acquisition |
| Volatility 3 | Memory analysis |
| dwarf2json | Linux kernel symbol generation |

The target VM is used for the controlled experiment and memory acquisition. REMnux is used to examine the acquired image independently.

### Suggested target configuration

- **Operating system:** Ubuntu 22.04 LTS.
- **Kernel:** Ubuntu GA 5.15 series, subject to verification of specimen compatibility.
- **Memory:** 2 GB minimum for this lab; increase if required by the target configuration.
- **Disk:** Approximately 40 GB, including space for tools and temporary acquisition artifacts.
- **Network:** Host-only or another appropriately isolated configuration.
- **Snapshot:** A clean snapshot taken before the experiment.
- **Secure Boot:** Disabled if required for the controlled kernel-module experiment.

Record the actual kernel version using `uname -r`. Do not assume that every Ubuntu 22.04 installation uses the same kernel: HWE kernels and subsequent updates may change the environment.

The forensic workstation must have enough free disk space to store the memory image, debugging information, symbol files, and analysis output.

## Prerequisites

Before starting, prepare:

- VMware or VirtualBox.
- A REMnux VM.
- A separate Ubuntu Linux target VM.
- Administrative access to the target VM.
- Internet access during the preparation phase, if required to retrieve tools and debugging packages.
- A clean target snapshot.
- Sufficient disk space for memory acquisition and analysis.
- Basic familiarity with Linux commands, kernel modules, and virtual machines.

The target must be isolated before loading the research specimen. Avoid bridged networking, shared folders, shared clipboard, and unnecessary host integrations.

## Tools and References

| Tool | Purpose | Official source |
|---|---|---|
| Diamorphine | Research specimen | [GitHub](https://github.com/m0nad/Diamorphine) |
| AVML | Memory acquisition | [GitHub](https://github.com/microsoft/avml) |
| Volatility 3 | Memory forensics | [GitHub](https://github.com/volatilityfoundation/volatility3) |
| dwarf2json | Linux symbol generation | [GitHub](https://github.com/volatilityfoundation/dwarf2json) |
| REMnux | Analysis environment | [Documentation](https://docs.remnux.org/) |

Use the upstream repositories to obtain the source code or releases. Record the exact versions or commit identifiers used in the experiment.

## Methodology

### 1. Prepare the Target VM

Update the target during the preparation phase and install the basic build dependencies and headers for the running kernel.

```bash
sudo apt update
sudo apt install -y build-essential linux-headers-$(uname -r) git
```

Record the operating-system release and kernel version:

```bash
cat /etc/os-release
uname -a
uname -r
uname -m
```

Check whether the matching kernel debugging information is available before proceeding to symbol preparation.

The kernel headers and debugging symbols serve different purposes: headers support module compilation, while debugging information is needed to generate a suitable Volatility symbol table.

Document any package availability issues rather than silently changing the target kernel.

### 2. Obtain the Research Specimen

Retrieve Diamorphine from its original upstream repository:

```bash
git clone https://github.com/m0nad/Diamorphine.git
```

Record the repository revision:

```bash
cd Diamorphine
git rev-parse HEAD
```

Return to the lab directory when finished.

The repository does not redistribute a separate copy of the source code. The experiment uses the upstream project as its documented source.

Record the acquisition date, commit identifier, target kernel version, and any compatibility issues encountered.

### 3. Prepare the Forensic Workstation

Verify that Volatility 3 is installed and that its Linux plugins are available.

For a source installation, consult the official installation instructions:

[Volatility 3 repository](https://github.com/volatilityfoundation/volatility3)

For example, if Volatility 3 is installed in a Python virtual environment:

```bash
python3 -m venv volatility-env
source volatility-env/bin/activate
python -m pip install volatility3
```

Check the available commands:

```bash
vol -h
vol --help | grep -i linux
```

Depending on the installation, the executable may instead be `vol.py` or another wrapper. Use the command appropriate for the installed version.

Install or obtain `dwarf2json` using the official project instructions. Record the installed versions of Volatility 3 and `dwarf2json`.

### 4. Isolate the Environment

Before conducting the experiment:

1. Shut down the target VM.
2. Configure an isolated network.
3. Disable unnecessary shared host integrations.
4. Start the VM and verify its network configuration.
5. Create a snapshot named `clean`.

Disconnect Internet access once the required packages and tools have been obtained.

The host-only network should not provide an unintended route to production systems or the public Internet. Verify the actual network configuration instead of relying solely on a failed ping test.

### 5. Collect the Baseline

Before introducing the specimen, record the target's initial state.

```bash
uname -a
cat /proc/sys/kernel/tainted
lsmod
ls /sys/module
ps -e -o pid,ppid,comm
```

Save the outputs to a local evidence directory. Record the time of collection and the exact commands used.

The baseline should include:

- Kernel version and architecture.
- Kernel taint value.
- Visible kernel modules.
- Process identifiers and names.
- Relevant system configuration.
- Any unexpected output or environmental warnings.

These measurements establish the reference state for the later comparison.

### 6. Conduct the Controlled Experiment

Use the disposable target VM and follow the documented upstream project instructions for the controlled experiment.

Record the specimen version, build outcome, kernel version, relevant console messages, and any changes observed in process or module visibility.

Do not assume that behavior documented for one kernel version will be identical on another. If the specimen does not compile or the expected behavior is not observed, preserve the error messages and document the result.

Repeat the baseline measurements after the experiment using the same commands.

Treat the results as observations rather than proof of compromise or successful concealment until they have been independently examined.

### 7. Acquire the Memory Image

AVML is used to acquire the target's volatile memory for offline analysis.

Obtain the appropriate AVML release from the official repository and verify its provenance before using it.

Record the following acquisition metadata:

- Acquisition date and time.
- Target hostname and kernel version.
- AVML version.
- Output format.
- Output file size.
- SHA-256 hash.
- Any errors or warnings.

For example, calculate the hash of the acquired image with:

```bash
sha256sum mem.lime
```

Use the actual output filename produced by the acquisition tool; the name `mem.lime` is only an example.

Transfer the image to the forensic workstation over the isolated lab network or use an appropriately controlled offline transfer method.

Verify the hash after transfer:

```bash
sha256sum mem.lime
```

The source and destination hashes should match.

**The full memory image is a local experimental artifact.**


### 8. Identify the Target Kernel

Use Volatility 3 to identify kernel banners in the acquired image.

For installations using the `vol` command:

```bash
vol -f mem.lime banners
```

Use the appropriate plugin name and command syntax for the installed Volatility 3 version.

Record the discovered kernel banner and compare it with the target's previously recorded kernel version.

A banner helps identify the kernel but does not, by itself, guarantee that a symbol table is compatible.

### 9. Prepare Linux Kernel Symbols

Volatility 3 requires suitable Linux symbol information to interpret kernel structures reliably.

First, check whether an appropriate pre-generated symbol table is available for the target kernel. Consult the official [Volatility 3 Linux tutorial](https://volatility3.readthedocs.io/en/latest/getting-started-linux-tutorial.html).

If a suitable symbol table is unavailable, obtain the matching kernel debugging information and follow the official `dwarf2json` instructions to generate the required Intermediate Symbol Format (ISF).

The exact procedure depends on the target distribution, kernel build, package availability, and the installed versions of the tools.

Record:

- The exact target kernel release.
- The source of the debugging information.
- The source package or build identifier, where available.
- The `dwarf2json` version.
- The generated symbol file's name and SHA-256 hash.
- Any symbol-resolution errors reported by Volatility 3.

Place the resulting symbol file in the symbol directory recognized by the installed Volatility 3 configuration.

Do not substitute an arbitrary symbol table merely because it belongs to the same distribution or major kernel series.

### 10. Analyze the Memory Image

Use the plugins available in the installed Volatility 3 version to investigate process enumeration, kernel modules, and relevant kernel structures.

Begin by listing the Linux plugins:

```bash
vol --help | grep -i linux
```

Examples of commonly documented Linux plugins include:

```bash
vol -f mem.lime linux.pslist
vol -f mem.lime linux.pstree
vol -f mem.lime linux.lsmod
```

The precise plugin availability and naming depend on the installed release. Check the local help output before relying on any particular plugin.

Where supported, examine additional information relevant to the investigation, such as kernel messages or credential structures.

Save the output of each command to a separate evidence file. Record the command, tool version, symbol configuration, and any warnings alongside the output.

For example:

```bash
mkdir -p evidence/volatility
```

The memory image referenced in these examples must remain on the local analysis workstation and outside the public repository.

### 11. Compare the Evidence

Compare the initial baseline, post-experiment observations, and offline memory-analysis results.

Investigate:

- Processes present in one enumeration but absent from another.
- Differences between the visible module list and other available kernel artifacts.
- Unexpected kernel state or taint changes.
- Symbol-resolution failures.
- Inconsistent results across plugins.
- Environmental factors that could explain the observations.

A discrepancy is a lead for further investigation, not automatic proof of rootkit activity.

A plugin may fail to identify an artifact because its detection method does not cover the relevant behavior, the required symbols are unavailable, or the acquired image is incomplete or incompatible.

Document both successful and unsuccessful analysis attempts.

### 12. Findings and Validation

For every finding, record the observation, supporting evidence, alternative explanations, and confidence level.

Use the following categories:

| Classification | Meaning |
|---|---|
| Confirmed | Supported by reproducible evidence and appropriate validation. |
| Suspected | Consistent with rootkit activity but not independently verified. |
| Inconclusive | Available evidence does not support a reliable conclusion. |

Do not claim successful detection or concealment until the relevant behavior has been observed in the actual environment.

Record compatibility failures and negative results. A clean plugin output does not establish that the target is free from compromise.

### 13. Cleanup

After collecting the required evidence:

1. Stop the experiment.
2. Shut down the target VM.
3. Restore the `clean` snapshot.
4. Verify the restored target before reusing it.
5. Securely remove temporary files when they are no longer required, following your storage and evidence-handling procedures.

Retain only the sanitized outputs and metadata required for the public write-up.

## Evidence and Repository Structure

The lab repository contains documentation and sanitized evidence, not the full memory image.

```text
001-linux-rootkit-memory-forensics/
├── README.md
├── notes/
│   ├── environment.md
│   └── findings.md
├── screenshots/
├── scripts/
├── evidence/
│   ├── baseline/
│   └── volatility/
└── references.md
```

The contents may evolve as the investigation progresses.

Suitable public artifacts include:

- Sanitized baseline measurements.
- Sanitized post-experiment measurements.
- Volatility 3 output.
- Kernel and tool version information.
- Hashes and acquisition metadata.
- Screenshots and annotated comparisons.
- Troubleshooting notes and known limitations.


## Results

**Status: Pending experimental validation.**

Results will be added after the experiment has been executed and the evidence reviewed.

The final analysis will compare baseline observations with the acquired memory image, assess which discrepancies can be reproduced, and document the effectiveness and limitations of the selected detection methods.

No detection result should be considered confirmed until it has been observed in the actual laboratory environment.

## Limitations

Results depend on the target kernel, kernel configuration, specimen compatibility, acquisition method, symbol accuracy, and Volatility 3 plugin implementation.

A memory acquisition may not contain every artifact needed for a particular analysis. Symbol mismatches can cause plugins to fail or produce incomplete results.

The visibility of processes and kernel modules may differ between tools, and not every discrepancy is malicious.

This experiment evaluates selected forensic techniques. It does not constitute a comprehensive assessment of Linux rootkit detection or guarantee that all forms of kernel compromise can be identified.

## Safety and Cleanup

- Use disposable virtual machines and clean snapshots.
- Keep the target isolated from production networks and sensitive systems.
- Avoid bridged networking.
- Disable unnecessary shared folders, clipboard integration, and host integrations.
- Do not expose credentials or personal files to the target VM.
- Keep full memory images and sensitive artifacts outside version control.
- Verify artifact hashes after acquisition and transfer.
- Restore the clean snapshot after the experiment.
- Publish only evidence that has been reviewed and sanitized.

Virtualization reduces risk but does not guarantee containment.

## References

- [Diamorphine](https://github.com/m0nad/Diamorphine)
- [Volatility 3](https://github.com/volatilityfoundation/volatility3)
- [Volatility 3 — Linux Tutorial](https://volatility3.readthedocs.io/en/latest/getting-started-linux-tutorial.html)
- [Volatility 3 — Symbol Tables](https://volatility3.readthedocs.io/en/develop/symbol-tables.html)
- [AVML](https://github.com/microsoft/avml)
- [dwarf2json](https://github.com/volatilityfoundation/dwarf2json)
- [REMnux Documentation](https://docs.remnux.org/)

---

*Part of `rem labs` — Learn by observing. Verify by reproducing.*