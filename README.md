# iCog Quantum Computing Task

This repository contains quantum computing exercises completed as part of an iCog Labs qualification task. It covers two fundamental quantum algorithms — **Quantum Teleportation** and the **Deutsch-Jozsa Algorithm** — through a hands-on simulation and accompanying reference documents.

---

## Repository Contents

| File | Type | Description |
|------|------|-------------|
| `Quantum_Teleportation_Simulation.ipynb` | Jupyter Notebook | Step-by-step simulation of the quantum teleportation protocol using Qiskit |
| `Quantum Teleportation.pdf` | PDF | Theoretical background and explanation of the quantum teleportation protocol |
| `Deutsch-Jozsa Algorithm Summary.pdf` | PDF | Summary of the Deutsch-Jozsa quantum algorithm |

---

## Quantum Teleportation Simulation

### Overview

`Quantum_Teleportation_Simulation.ipynb` implements the **quantum teleportation protocol** — a foundational quantum computing technique that transfers an arbitrary quantum state from one qubit to another using a shared entangled pair and classical communication, without physically moving the qubit.

### Prerequisites

- Python 3
- [Qiskit](https://qiskit.org/) v2.0.3
- [Qiskit Aer](https://github.com/Qiskit/qiskit-aer) v0.17.1

Install dependencies by running the first cell of the notebook, or manually:

```bash
pip install qiskit qiskit-aer
```

### How to Run

1. Launch Jupyter Notebook or JupyterLab:
   ```bash
   jupyter notebook Quantum_Teleportation_Simulation.ipynb
   ```
2. Run all cells in order (Cell → Run All).

### Circuit Design

The simulation uses a **3-qubit, 3-classical-bit** circuit and proceeds through the following stages:

#### Stage 1 — State Preparation
Qubit 0 (the sender's qubit) is flipped to state |1⟩ using an **X gate**. This is the state we want to teleport.

```
q0: ─X─
```

#### Stage 2 — Bell Pair (Entanglement)
Qubits 1 and 2 are entangled to form a **Bell pair** using a Hadamard gate followed by a CNOT gate. Qubit 1 belongs to the sender (Alice) and qubit 2 to the receiver (Bob).

```
q1: ─H─●─
       │
q2: ───X─
```

#### Stage 3 — Bell Measurement
The sender entangles qubit 0 with qubit 1 (CNOT), applies a Hadamard to qubit 0, then measures both qubits 0 and 1 into classical bits 0 and 1.

```
q0: ─●─H─┤M├
     │
q1: ─X───┤M├
```

#### Stage 4 — Classical Correction
Based on the two classical measurement results, conditional gates are applied to qubit 2 to recover the original state:
- If classical bit 1 is `1` → apply **CNOT** to qubit 2 (controlled by classical bit 1)
- If classical bit 0 is `1` → apply **Z gate** to qubit 2 (controlled by classical bit 0)

```
q1: ─●─
     │
q0: ─●─
     │
q2: ─X─Z─
```

#### Stage 5 — Verification
Qubit 2 is measured and the circuit is executed on the **Aer Simulator** for 1000 shots. A histogram of measurement outcomes confirms that the teleported state matches the original state |1⟩.

### Expected Output

The histogram produced by the simulation should show that qubit 2's measurement (the rightmost bit in the output string) consistently produces `1`, corresponding to the successfully teleported state |1⟩.

### Quantum Gates Used

| Gate | Symbol | Description |
|------|--------|-------------|
| X (Pauli-X) | X | Bit-flip; maps \|0⟩ → \|1⟩ and \|1⟩ → \|0⟩ |
| H (Hadamard) | H | Creates superposition; maps \|0⟩ → (\|0⟩+\|1⟩)/√2 |
| CNOT (CX) | ●─X | Controlled-NOT; flips target qubit if control is \|1⟩ |
| CZ | ●─Z | Controlled-Z; applies Z phase to target if control is \|1⟩ |
| Measure | M | Projects quantum state to a classical bit |

---

## Reference Documents

### Quantum Teleportation.pdf
Provides the theoretical foundation for the quantum teleportation protocol implemented in the notebook, including the mathematical formalism, circuit derivation, and explanation of why entanglement and classical communication together enable faithful state transfer.

### Deutsch-Jozsa Algorithm Summary.pdf
Summarizes the **Deutsch-Jozsa algorithm** — one of the earliest examples of a quantum algorithm that exponentially outperforms any deterministic classical algorithm. It solves the problem of determining whether a given black-box (oracle) function is *constant* (always outputs 0 or always outputs 1) or *balanced* (outputs 0 for half the inputs and 1 for the other half) in a single query, compared to up to 2^(n-1)+1 classical queries.

---

## Key Concepts

- **Quantum Entanglement**: A phenomenon where two qubits share a correlated quantum state such that measuring one instantly determines the state of the other, regardless of distance.
- **Bell States**: Maximally entangled two-qubit states, used as the entangled resource in quantum teleportation.
- **Quantum Superposition**: A qubit can exist in a combination of |0⟩ and |1⟩ states simultaneously until measured.
- **No-Cloning Theorem**: It is impossible to create an identical copy of an arbitrary unknown quantum state — teleportation moves the state rather than copying it.
- **Classical Communication**: Teleportation requires 2 classical bits to be sent from sender to receiver to complete the protocol.

---

## Technologies

- **[Qiskit](https://qiskit.org/)** — IBM's open-source quantum computing SDK for Python
- **[Qiskit Aer](https://github.com/Qiskit/qiskit-aer)** — High-performance quantum circuit simulator
- **Jupyter Notebook** — Interactive Python environment for running and visualizing the simulation
