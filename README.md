# Hybrid Classical-Quantum Bell-State Optimizers

This repository contains two related hybrid classical-quantum simulators for two-qubit Bell-state preparation.

The two notebooks are intentionally kept as separate versions because they represent different levels of physical and engineering complexity:

- **V1** is the ideal, transparent baseline: target state -> parameterized circuit -> objective function -> classical optimization -> final-state verification.
- **V2** extends the same workflow to density-matrix dynamics, hardware errors, finite-shot measurements, state estimation, and classical feedback control.

Together they show the progression from an ideal hybrid optimization loop to a hardware-aware, finite-resource control problem.

> Equations in this README are written in plain text rather than LaTeX so the file remains readable in GitHub, Notepad, and other plain-text editors.

---

## Repository Contents

```text
.
├── sim_hybrid_opti_Bellstate_prep.ipynb
├── sim_hybrid_opti_Bellstate_V2.ipynb
├── sim_hybrid_opti_Bellstate_prep_brief.pdf
├── hybrid_quantum_classical_control_technical_brief.pdf
├── README.md
├── requirements.txt
└── LICENSE
```

The repository uses the MIT License.

---

## Common Problem

Both simulators address the same basic task:

```text
target state
    ->
parameterized quantum system
    ->
quantitative objective
    ->
classical optimization / control
    ->
verification against the target
```

The difference is how much of the underlying physics and hardware behavior is represented.

V1 establishes a minimal, inspectable reference implementation. V2 preserves that baseline while adding realistic state representations, noise, finite measurement resources, and closed-loop recovery.

---

## V1 - Ideal Bell-State Preparation

Notebook:

```text
sim_hybrid_opti_Bellstate_prep.ipynb
```

V1 implements a compact hybrid classical-quantum workflow for preparing a Bell-like two-qubit state with a parameterized quantum circuit and a classical optimizer.

The goal is not to provide a production optimizer. The goal is to demonstrate a transparent and inspectable workflow in which each modeling assumption and numerical result can be checked directly.

### V1 Workflow

```text
target state
    ->
parameterized circuit
    ->
objective function
    ->
classical optimizer
    ->
final-state verification
```

### V1 Capabilities

- Bell-state target definition
- parameterized two-qubit circuit construction
- classical optimization of a circuit parameter
- fidelity-based cost function
- convergence tracking
- final-state verification
- concurrence calculation
- target-vs-optimized amplitude comparison
- optimized circuit parameter reporting
- function-evaluation and iteration-count reporting

### V1 Model

The simulator optimizes a parameterized state-preparation circuit against a Bell-state target.

The objective is based on target-state fidelity:

```text
cost = 1 - fidelity
```

A successful optimization produces final fidelity close to one and concurrence close to the target concurrence.

### V1 Role

V1 is deliberately idealized. It provides the clean reference case before adding:

- decoherence,
- finite-shot sampling,
- measurement uncertainty,
- calibration errors,
- hardware-specific constraints,
- resource accounting,
- state-estimation uncertainty.

This makes V1 useful as a frozen regression baseline for later extensions.

---

## V2 - Hardware-Aware Hybrid Control

Notebook:

```text
sim_hybrid_opti_Bellstate_V2.ipynb
```

V2 extends the ideal Bell-state preparation problem into a finite-resource hybrid control problem.

The simulator is designed as a technical scaffold for exploring how a classical controller can identify and compensate recoverable coherent hardware errors while respecting irreducible limits imposed by decoherence and finite measurement resources.

### V2 Target Family

The ideal two-parameter target family is

```text
|psi(alpha, phi)> = cos(alpha/2)|00> + exp(i phi) sin(alpha/2)|11>
```

For this family,

```text
C = |sin(alpha)|
```

where `C` is the concurrence.

Concurrence determines the entanglement amplitude but is insensitive to the relative phase `phi`. V2 therefore distinguishes entanglement matching from full target-state preparation through target-state fidelity.

### V2 Hardware-Control Model

Systematic control offsets are represented as

```text
alpha_actual = alpha_command + delta_alpha
phi_actual   = phi_command   + delta_phi
```

The controller therefore operates on commanded parameters while the simulated hardware applies biased parameters.

### V2 Capabilities

- density-matrix simulation of the two-qubit Bell-state circuit
- Wootters concurrence for arbitrary two-qubit density matrices
- pure-target fidelity evaluation
- local CPTP noise channels:
  - amplitude damping / relaxation
  - pure dephasing
  - depolarizing noise
- explicit placement of noise at defined locations in the circuit
- two-parameter control using `alpha` and `phi`
- exact-state and finite-shot hybrid optimization
- full two-qubit Pauli tomography using 15 nontrivial observables
- model-aware X-state reconstruction using 7 observables:
  - `ZI`
  - `IZ`
  - `ZZ`
  - `XX`
  - `YY`
  - `XY`
  - `YX`
- systematic hardware calibration offsets
- finite-shot measurement and state reconstruction
- separation of recoverable coherent error from irreducible decoherence
- explicit shot-resource accounting
- interactive dashboard comparing:
  - nominal hardware performance
  - hybrid-controlled performance
  - noisy-hardware ceiling
  - ideal target

---

## V1 to V2 Progression

| Capability | V1 | V2 |
|---|---|---|
| State representation | Pure state vector | Density matrix |
| Control variables | Single circuit parameter | `alpha`, `phi` |
| Relative phase control | No | Yes |
| Noise model | None | CPTP channels |
| Calibration offsets | None | Explicit control bias |
| Measurement model | Exact state access | Finite-shot Pauli measurements |
| State estimation | None | Tomographic reconstruction |
| Optimization | Ideal target preparation | Hardware-aware hybrid feedback |
| Concurrence | Exact diagnostic | Exact and estimated |
| Fidelity | Exact diagnostic | Exact and estimated |
| Physical performance ceiling | Not modeled | Explicitly computed |
| Recoverable vs irreducible error | Not separated | Explicitly separated |
| Measurement-resource accounting | No | Yes |
| Reduced measurement model | N/A | 15 -> 7 observables |
| Verification | Ideal analytic checks | Layered unit-test and regression stack |

The progression is intentional:

```text
V1: ideal hybrid optimization
    ->
V2: noisy, finite-shot, hardware-aware hybrid control
```

---

## V2 Validation Strategy

V2 was developed cell-by-cell. Each computational layer was validated before the next layer was introduced.

The frozen test stack verifies:

1. density-matrix reproduction of the ideal pure-state circuit,
2. trace preservation, Hermiticity, positivity, and purity,
3. analytic concurrence limits for the implemented noise channels,
4. location-dependent effects of noise in the circuit,
5. phase-insensitivity of concurrence and phase-sensitivity of fidelity,
6. exact and finite-shot state reconstruction,
7. finite-shot hybrid optimization using estimated metrics only,
8. recovery of coherent control errors without exceeding the noisy-hardware ceiling,
9. exact X-state reconstruction from the reduced observable set,
10. dashboard reproduction of the frozen benchmark.

This creates a verification ladder:

```text
analytic physics
    ->
numerical primitive
    ->
composed circuit
    ->
measurement model
    ->
state estimator
    ->
classical controller
    ->
system benchmark
```

Each validated layer serves as a regression target for later development.

---

## Validated V2 Benchmark

The frozen validation case uses:

```text
target concurrence      = 1.00
target phase            = 0.00
delta_alpha             = 0.25
delta_phi               = 0.70
post-CNOT dephasing q0  = 0.20
post-CNOT dephasing q1  = 0.00
shots per observable    = 2000
```

Validated results:

| Quantity | Value |
|---|---:|
| Nominal fidelity | 0.796426 |
| Hybrid-controlled fidelity | 0.898022 |
| Noisy-hardware fidelity ceiling | 0.900000 |
| Ideal fidelity | 1.000000 |
| Nominal concurrence | 0.775130 |
| Hybrid-controlled concurrence | 0.798093 |
| Noisy-hardware concurrence ceiling | 0.800000 |
| Recoverable fidelity | 0.103574 |
| Fidelity recovered | 0.101596 |
| Recovery fraction | 98.1% |
| Controller gap | 0.001978 |
| Irreducible fidelity gap | 0.100000 |

The benchmark demonstrates the central V2 distinction:

```text
coherent calibration error
    ->
potentially recoverable by retuning

decoherence
    ->
irreducible physical performance ceiling
```

For this case, the finite-shot controller restores 98.1% of the recoverable fidelity loss while remaining below the exact noisy-hardware ceiling.

---

## Measurement Compression in V2

Full two-qubit Pauli tomography uses 15 nontrivial Pauli observables.

For the current circuit and noise family, the reachable state remains in the two-qubit X-state family. This allows model-aware reconstruction using only:

```text
ZI, IZ, ZZ, XX, YY, XY, YX
```

The observable count is therefore reduced from:

```text
15 -> 7
```

For the frozen 659-evaluation benchmark:

```text
full-tomography shot count = 19,770,000
reduced shot count         =  9,226,000
```

This corresponds to a 53.3% reduction under the current per-observable accounting.

The reduced 7-observable reconstruction is model-specific. It is not a universal two-qubit tomography scheme.

---

## Installation

A clean Python environment is recommended.

### Option 1 - Conda

```bash
conda create -n hybrid-bell-optimizer python=3.11 -y
conda activate hybrid-bell-optimizer

python -m pip install --upgrade pip
pip install -r requirements.txt
```

### Option 2 - Python venv

Linux/macOS:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Windows:

```text
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```

Launch JupyterLab:

```bash
jupyter lab
```

---

## Running V1

Open:

```text
sim_hybrid_opti_Bellstate_prep.ipynb
```

Run the notebook from top to bottom.

The notebook produces:

- optimization trace,
- parameter stabilization,
- target-vs-optimized state comparison,
- final fidelity,
- target and final concurrence,
- optimized circuit parameter,
- function-evaluation and iteration information.

V1 can be run independently of V2.

---

## Running V2

Open:

```text
sim_hybrid_opti_Bellstate_V2.ipynb
```

Run the notebook from top to bottom.

Each validation cell should report:

```text
PASS
```

before proceeding to the next layer.

The final dashboard exposes the full V2 engineering workflow:

```text
target specification
    ->
hardware imperfections
    ->
finite-shot measurements
    ->
state reconstruction
    ->
classical control
    ->
retuned hardware
    ->
comparison with physical ceiling
```

Two search modes are available in the dashboard:

- **Fast demo** for rapid interactive use
- **Validated benchmark** for reproduction of the frozen regression case

---

## Scope and Limitations

### V1

V1 intentionally excludes:

- finite-shot sampling,
- gate noise,
- decoherence,
- readout error,
- hardware-native gate constraints,
- pulse-level modeling,
- time-to-solution estimates.

Its purpose is to preserve a minimal and transparent ideal reference.

### V2

V2 is intentionally small and auditable.

Current limitations include:

- two-qubit Bell-like state family,
- coarse-to-fine derivative-free controller,
- simplified local noise channels,
- no explicit readout-error model,
- no leakage channel,
- no pulse-level dynamics,
- no direct hardware execution,
- no online drift model,
- no adaptive shot allocation.

The present amplitude-damping channel models qubit relaxation.

For photonic dual-rail hardware, photon loss should instead be represented explicitly as leakage or erasure outside the logical qubit subspace rather than identified directly with abstract-qubit amplitude damping.

---

## Development Philosophy

The repository follows a simple development rule:

```text
small model
    ->
analytic or known validation target
    ->
unit test
    ->
freeze validated layer
    ->
add one new capability
```

This makes the numerical development auditable and helps localize failures when later layers are modified.

The same strategy is intended for future architectures and larger models.

---

## Planned Extensions

The current V2 architecture is designed so that individual layers can be replaced independently.

Near-term directions include:

- prior characterization loaded from a resource file,
- uncertainty-aware warm-start control,
- robust optimization under incomplete state or hardware information,
- adaptive measurement selection,
- adaptive shot allocation,
- hardware-specific noise and calibration models,
- explicit leakage / erasure channels,
- online drift estimation,
- SPSA, CMA-ES, Bayesian optimization, or reinforcement-learning controllers,
- larger parameterized circuits,
- direct hardware or cloud-backend execution,
- error mitigation and error-correction layers.

A longer-term direction is a reusable multi-architecture framework in which the same hybrid control problem can be evaluated with architecture-specific backends such as:

```text
DV quantum backend
classical photonic backend
quantum photonic backend
```

The goal is to keep control, estimation, orchestration, logging, and validation reusable while isolating architecture-specific physics behind validated backend interfaces.

---

## Technical Briefs

Two technical briefs accompany the notebooks:

```text
sim_hybrid_opti_Bellstate_prep_brief.pdf
hybrid_quantum_classical_control_technical_brief.pdf
```

The V1 brief documents the ideal hybrid state-preparation workflow.

The V2 brief documents:

- density-matrix formulation,
- CPTP noise channels,
- finite-shot measurement,
- state reconstruction,
- hybrid recovery,
- physical performance ceilings,
- measurement compression,
- verified benchmark results,
- limitations and extension pathways.

---

## Relevance

The repository demonstrates a progression from a compact ideal quantum workflow to a hardware-aware hybrid control problem.

The main technical themes are:

- explicit target-state definition,
- hybrid classical-quantum optimization,
- density-matrix simulation,
- quantum-noise modeling,
- finite-shot measurement,
- state estimation,
- recoverability analysis,
- resource-aware control,
- unit-test-driven scientific development,
- reusable architecture design.

The Bell-state problem is deliberately small enough that the underlying physics remains inspectable while still supporting meaningful entanglement, noise, measurement, and feedback-control behavior.

---

## License

This project is released under the MIT License.

See:

```text
LICENSE
```

for the full license text.

---

## Author

**Dr. Boris Kiefer**  
New Mexico State University  
GitHub: [boriskiefer](https://github.com/boriskiefer)
