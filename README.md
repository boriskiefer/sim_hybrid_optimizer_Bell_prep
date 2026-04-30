# Hybrid Classical-Quantum Optimizer for Bell-State Preparation

This repository contains a compact hybrid classical-quantum workflow for preparing a Bell state with a parameterized quantum circuit and a classical optimizer.

The goal is not to provide a production optimizer. The goal is to demonstrate a transparent, inspectable workflow for quantum state preparation:

```text
target state → parameterized circuit → objective function → classical optimization → final fidelity
```

The example is intentionally small. It is designed as a portfolio artifact showing how a quantum workflow can be scoped, implemented, validated, visualized, and extended toward more realistic hardware-aware simulations.

## Motivation

Hybrid classical-quantum workflows are central to many near-term quantum algorithms. Even in a minimal state-preparation example, the same workflow pattern appears:

1. define a target quantum state,
2. construct a parameterized circuit,
3. evaluate an objective function,
4. update parameters with a classical optimizer,
5. verify the final state using quantitative metrics.

This project applies that pattern to Bell-state preparation. The optimizer searches for a circuit parameter that prepares a target entangled two-qubit state with high fidelity.

The notebook emphasizes clarity over complexity. Each step is written to make the modeling assumptions explicit.

## What the Demo Shows

The current version demonstrates:

* Bell-state target definition
* parameterized two-qubit circuit construction
* classical optimization of a circuit parameter
* fidelity-based cost function
* convergence tracking
* final-state verification
* concurrence calculation
* compact visual comparison of target and optimized state amplitudes

The output includes:

* optimization trace
* parameter stabilization
* target-vs-optimized amplitude comparison
* final fidelity
* target and final concurrence
* optimized circuit parameter
* function evaluations and iteration count

## Repository Structure

```text
hybrid-classical-quantum-state-preparation/
├── README.md
├── sim_hybrid_opti_Bellstate_prep.ipynb
├── sim_hybrid_opti_Bellstate_prep_brief.pdf
├── requirements.txt
```

Depending on how the repository is organized, the notebook may also appear in a `notebooks/` directory.

## Installation

A clean conda environment is recommended.

```bash
conda create -n hybrid-bell-optimizer python=3.11 -y
conda activate hybrid-bell-optimizer

python -m pip install --upgrade pip
pip install -r requirements.txt
```

A minimal `requirements.txt` is:

```text
numpy
scipy
matplotlib
jupyterlab
```

If the notebook is later adapted to use Qiskit explicitly, add:

```text
qiskit
```

## Running the Notebook

Start JupyterLab:

```bash
jupyter lab
```

Then open the notebook and run all cells.

The notebook should execute end-to-end and produce:

1. a one-row, three-panel plot showing optimization behavior,
2. a compact numerical verification table,
3. printed target and optimized state vectors.

## Model Description

The workflow optimizes a parameterized state-preparation circuit against a Bell-state target.

The cost function is:

```text
cost = 1 - fidelity
```

where the fidelity measures overlap between the optimized final state and the target Bell state.

The optimizer minimizes this cost by adjusting the circuit parameter. A successful run should produce final fidelity close to one and concurrence close to the target concurrence.

## Interpretation

This project is best understood as a minimal hybrid workflow demonstrator.

It shows the structure of a quantum application prototype:

```text
quantum objective → classical optimization loop → numerical verification → performance summary
```

In a larger setting, the same structure can be extended to include:

* noisy state preparation
* finite-shot sampling
* measurement uncertainty
* hardware-specific constraints
* calibration-sensitive parameters
* runtime and sampling-cost estimates
* hardware-aware performance modeling

The current version is deliberately idealized. It provides the clean baseline before adding noise and device constraints.

## Relevance

This repository demonstrates several skills relevant to quantum applications and performance modeling:

* building compact quantum simulation workflows
* defining explicit target states and objective functions
* connecting algorithmic parameters to measurable outcomes
* tracking convergence and optimization behavior
* validating results with fidelity and entanglement metrics
* producing readable, reproducible scientific code

The example uses Bell-state preparation because it is simple enough to inspect directly while still involving genuine two-qubit entanglement.

## Current Limitations

The current version does not yet include:

* finite-shot sampling
* gate noise
* decoherence
* readout error
* hardware-native gate constraints
* pulse-level modeling
* time-to-solution estimates

These are natural extensions. The present notebook establishes the transparent baseline needed before adding those effects.

## Planned Extensions

Possible next steps include:

1. add finite-shot sampling,
2. add depolarizing or amplitude-damping noise,
3. compare ideal and noisy optimization traces,
4. include readout uncertainty,
5. estimate sampling cost for target fidelity thresholds,
6. adapt the workflow to hardware-native gate sets,
7. compare optimizer behavior under different noise assumptions.

## Portfolio Context

This project was developed as a compact portfolio artifact demonstrating rapid construction of an inspectable hybrid classical-quantum workflow.

It complements other quantum workflow examples, including Bell-state measurement, entanglement swapping, quantum networking, and hardware-aware photonic simulation.

The broader theme is transparent quantum performance modeling: start from a physical or algorithmic target, define the minimal useful model, expose assumptions, compute quantitative metrics, and communicate the result clearly.

## License

This repository uses the MIT License, selected from GitHub standard license templates. See the LICENSE file in this repository for the full license text.

## Author

**Dr. Boris Kiefer**  
New Mexico State University  
GitHub: [boriskiefer](https://github.com/boriskiefer)

