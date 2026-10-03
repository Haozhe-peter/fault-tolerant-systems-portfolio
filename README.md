# Trust-Algorithm Fault Injection & TMR Verification

**Explore how corrupted internal state affects trust decisions—and how a majority voter behaves when one input disagrees.**

This dependable-systems team project contains C++ trust-model implementations for high-level synthesis, selectable state-bit fault injection, and a separate Vivado triple-modular-redundancy (TMR) demonstration.

`C++` · `Vitis HLS` · `Verilog testbench` · `Vivado` · `RTL simulation`

## My contribution

Generated simulation waveform images to document signal behavior. My role focused on waveform visualization; the implementations and simulation results below are team-level work.

## The engineering problem

A trust algorithm keeps state used to decide whether a node should be trusted. Corrupting that state can change the decision even when the program continues to run. The project exposes a controlled way to flip selected state bits and inspect the resulting behavior.

TMR addresses a related problem by comparing three redundant results. When two correct results agree and the third differs, a majority voter can preserve the expected output. This depends on the fault model; it is not a guarantee against every failure.

## Team implementation

- **Four HLS trust-model variants:** `trust_model_a`, `trust_model_g`, `trust_model_m`, and `trust_model_t`.
- **Selectable fault injection:** a node identifier and XOR mask select which stored bits to flip.
- **Explicit operation modes:** reset, hold, fault injection, and update, followed by output evaluation.
- **C++ testbenches:** expected-output checks for model behavior; the Model A testbench includes visible-fault and hidden-fault cases.
- **A separate RTL voting demonstration:** a Verilog testbench drives three paths and checks the majority-voted result.

The HLS models and RTL voting demonstration are documented as separate components, not as a verified end-to-end deployment.

## Team simulation result

**256 of 256 checks passed in the archived Vivado voting test.**

The testbench holds one input at zero, sweeps the other two together from 0 to 255, and expects the voted output to equal the swept value plus one. The saved simulation log reports:

```text
PASS COUNT: 256/256
FAIL COUNT: 0/256
```

**Scope:** this archived result covers one add-one/voter scenario, not all fault patterns or overall system reliability. It has not been rerun for this portfolio.

## Archived HLS synthesis estimates

<details>
<summary>View resource and latency estimates for the four model variants</summary>

These numbers come from the supplied `top_csynth.rpt` files, generated with Vitis HLS 2025.1 for `xc7v2000t-fhg1761-2` with a 10 ns target clock.

| Model | Latency, cycles | LUTs | Flip-flops | DSPs |
| --- | ---: | ---: | ---: | ---: |
| A | 9–16 | 344 | 153 | 0 |
| G | 1–5 | 1,623 | 744 | 16 |
| M | 9 | 524 | 180 | 0 |
| T | 8 | 610 | 113 | 0 |

These are historical synthesis estimates for different model implementations, not measured board performance or a controlled comparison of equivalent algorithms. The reports have different generation dates; source-to-report reproducibility has not been revalidated.

</details>

## Evidence sources

The original project archive contains the voting testbench at `vivado/TMR.srcs/sim_1/new/testbench.v` and its saved output at `vivado/TMR.sim/sim_1/behav/xsim/simulate.log`. HLS estimates are recorded in each model's `hls/syn/report/top_csynth.rpt` file. These paths identify the private source artifacts; the files are not redistributed here.

## Next validation steps

- Rerun the model testbenches and synthesis flow in a documented toolchain.
- Sweep fault locations and bit masks, and distinguish changed outputs from masked faults.
- Extend voting tests to other disagreeing paths, multiple faults, and voter failure scenarios.
- Document how HLS model outputs are connected to the redundant system before claiming integrated fault tolerance.

## Project context

This is a team project. The original implementation remains private; this repository is a technical case study with my role identified above.

[Back to my profile](https://github.com/Haozhe-peter)
