# Findings and Analysis Notes

> **Lab:** 001 — Linux Rootkit Detection & Memory Forensics  
> **Status:** IN PROGRESS  
> **Research specimen:** Diamorphine  
> **Primary analysis framework:** Volatility 3

---

## 1. Investigation Summary

This document records the observations and conclusions obtained while investigating Linux rootkit detection through memory forensics.

The analysis compares the target's baseline state, observations collected during the controlled experiment, and artifacts recovered from the acquired memory image.

**Current status:** Experimental findings have not yet been validated.

No detection outcome should be treated as confirmed until it has been observed and supported by evidence from the actual laboratory environment.

## 2. Research Questions

The investigation addresses the following questions:

1. What differences can be observed between the baseline and post-experiment system state?
2. Do process-enumeration methods produce consistent results?
3. Do available forensic methods reveal discrepancies in kernel module visibility?
4. Can Volatility 3 analyze the acquired image using compatible Linux symbols?
5. Which findings can be independently verified?
6. Which limitations arise from the kernel version, acquisition method, symbols, or plugin implementation?

## 3. Baseline Observations

Record the initial system state before introducing the research specimen.

| Observation | Result | Evidence |
|---|---|---|
| Target kernel version | TBD | `evidence/baseline/` |
| Kernel taint value | TBD | `evidence/baseline/` |
| Visible process count | TBD | `evidence/baseline/` |
| Visible kernel module count | TBD | `evidence/baseline/` |
| Unexpected system behavior | TBD | Supporting notes, if any |

### Notes

Document relevant environmental details, background processes, and any unexpected observations.

Do not assume that process counts or module counts remain constant across runs. Differences may result from ordinary operating-system activity.

## 4. Post-Experiment Observations

Record observations collected after the controlled experiment.

| Observation | Result | Comparison with baseline |
|---|---|---|
| Kernel version | TBD | TBD |
| Kernel taint value | TBD | TBD |
| Visible process enumeration | TBD | TBD |
| Visible kernel module enumeration | TBD | TBD |
| Relevant kernel messages | TBD | TBD |
| Unexpected errors | TBD | TBD |

### Notes

Include the command or method used for each observation, the collection time, and the corresponding evidence file.

Distinguish between directly observed behavior and interpretations that require further validation.

## 5. Memory Acquisition

| Property | Result |
|---|---|
| Acquisition tool and version | TBD |
| Acquisition method | TBD |
| Output format | TBD |
| Image size | TBD |
| SHA-256 | TBD |
| Transfer integrity verified | TBD |
| Acquisition warnings or errors | TBD |

### Acquisition assessment

Document whether the acquisition completed successfully, whether the output was readable, and whether the image could be analyzed using the intended tools.

A successful acquisition command does not automatically establish that the image contains every artifact required for a particular investigation.

The full memory image is stored outside the public repository.

## 6. Symbol Validation

| Property | Result |
|---|---|
| Identified kernel banner | TBD |
| Symbol source | TBD |
| ISF generated or obtained | TBD |
| Symbol compatibility | TBD |
| Symbol-resolution errors | TBD |

### Notes

Record any mismatch between the target kernel and the available debugging information.

Distinguish symbol-related errors from genuine negative findings. An unsuccessful plugin execution is not evidence that an artifact is absent.

## 7. Volatility 3 Results

Record the actual plugins executed, their outputs, and any limitations encountered.

| Plugin | Purpose | Result | Evidence |
|---|---|---|---|
| `linux.pslist` | Process enumeration | Pending | `evidence/volatility/` |
| `linux.pstree` | Process relationships | Pending | `evidence/volatility/` |
| `linux.lsmod` | Kernel module enumeration | Pending | `evidence/volatility/` |
| Other verified plugins | Depends on plugin | Pending | `evidence/volatility/` |

Plugin availability and behavior depend on the installed Volatility 3 version. Confirm the actual plugin names and capabilities before recording a result.

For each plugin, document:

- The command executed.
- The Volatility 3 version.
- The symbol configuration used.
- Whether execution completed successfully.
- Relevant output and warnings.
- Any discrepancies requiring further investigation.

Do not interpret an empty output as proof of successful concealment or a clean system.

## 8. Comparative Analysis

Use this section to compare the baseline, post-experiment observations, and offline memory analysis.

| Research question | Observation | Interpretation | Confidence |
|---|---|---|---|
| Process enumeration differs between methods | TBD | TBD | Not assessed |
| Module visibility differs between methods | TBD | TBD | Not assessed |
| Kernel state changed | TBD | TBD | Not assessed |
| Memory image analyzed successfully | TBD | TBD | Not assessed |
| Symbols were compatible | TBD | TBD | Not assessed |

### Interpretation guidelines

- Establish whether the compared measurements were collected under equivalent conditions.
- Investigate benign explanations for discrepancies.
- Verify whether the relevant plugins support the required analysis.
- Preserve contradictory observations rather than excluding them.
- Separate confirmed behavior from assumptions about the specimen's internal mechanisms.

## 9. Finding Records

Create one record for each significant finding.

### Finding F-001 — [Title]

**Status:** Pending

**Classification:** Confirmed / Suspected / Inconclusive

**Observation:**

Describe what was directly observed.

**Evidence:**

List the relevant evidence files, commands, hashes, or screenshots.

**Expected behavior:**

Explain what was expected and why.

**Actual behavior:**

Record the result obtained during the experiment.

**Alternative explanations:**

Identify other plausible explanations, including environmental differences and tool limitations.

**Validation:**

Document any independent checks used to assess the finding.

**Conclusion:**

State what the evidence supports and what remains uncertain.

---

## 10. Limitations and Failed Attempts

Document unsuccessful approaches and technical constraints, including:

- Kernel-module compilation failures.
- Kernel-version incompatibilities.
- Missing debugging-symbol packages.
- ISF generation errors.
- Volatility 3 plugin failures.
- Incomplete or unreadable acquisition artifacts.
- Results that could not be reproduced.

For each issue, record the error, the relevant environment details, the troubleshooting steps attempted, and the outcome.

Negative results are part of the investigation and should not be omitted merely because they differ from expectations.

## 11. Final Assessment

**Status:** Pending experimental validation.

Complete this section after reviewing the evidence.

Summarize:

- Which observations were reproduced.
- Which discrepancies were independently verified.
- Which analysis methods were useful.
- Which results remained inconclusive.
- Which compatibility issues affected the experiment.
- What further validation would be necessary.

A lack of detected anomalies does not establish that the target is free from rootkit activity.

## 12. References

- [Diamorphine](https://github.com/m0nad/Diamorphine)
- [Volatility 3](https://github.com/volatilityfoundation/volatility3)
- [Volatility 3 Linux Tutorial](https://volatility3.readthedocs.io/en/latest/getting-started-linux-tutorial.html)
- [AVML](https://github.com/microsoft/avml)
- [dwarf2json](https://github.com/volatilityfoundation/dwarf2json)

---

*Part of `rem labs` — Learn by observing. Verify by reproducing.*