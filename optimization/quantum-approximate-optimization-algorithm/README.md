# Quantum Approximate Optimization Algorithm (QAOA)

The **Quantum Approximate Optimization Algorithm (QAOA)** is a **hybrid quantum-classical algorithm** used to find approximate solutions to **combinatorial optimization problems**.

The basic idea is to represent possible solutions as quantum states and use a parameterized quantum circuit to increase the probability of obtaining high-quality solutions. A classical optimizer repeatedly adjusts the parameters of the quantum circuit based on the measured results.

QAOA therefore combines:

- **Quantum computation** to represent and explore possible solutions.
- **Classical optimization** to determine suitable parameters for the quantum circuit.

---

## Core Idea

A combinatorial optimization problem generally involves finding the best solution from a large set of possible configurations.

For a problem with $n$ binary variables,

$$
x_i \in \{0,1\}
$$

there can be up to

$$
2^n
$$

possible configurations.

QAOA maps these possible configurations to the computational basis states of $n$ qubits. It then uses two alternating quantum operations:

- **Cost Hamiltonian** — represents the objective function of the optimization problem.
- **Mixer Hamiltonian** — allows the quantum state to explore different configurations.

The parameters controlling these operations are optimized using a classical optimization algorithm.

The overall process is:

**Encode the problem → Apply quantum operations → Measure → Evaluate → Optimize parameters → Repeat**

---

## Initial State

For a problem involving $n$ binary variables, $n$ qubits are used.

The qubits are initialized in the equal superposition state:

$$
|+\rangle^{\otimes n}
$$

where

$$
|+\rangle =
\frac{|0\rangle + |1\rangle}{\sqrt{2}}
$$

For $n$ qubits, this produces:

$$
|+\rangle^{\otimes n}
$$

$$
\frac{1}{\sqrt{2^n}}
\sum_{x\in\{0,1\}^n}|x\rangle
$$

Here, each computational basis state $|x\rangle$ represents one possible configuration or solution of the optimization problem.

---

## Cost Hamiltonian

The **cost Hamiltonian $H_C$** encodes the objective function of the optimization problem.

Each computational basis state $|x\rangle$ corresponds to a possible solution, and the cost Hamiltonian associates an objective value with that solution:

$$
H_C|x\rangle = C(x)|x\rangle
$$

where $C(x)$ represents the cost or objective value associated with solution $x$.

The corresponding cost unitary is:

$$
U_C(\gamma)
$$

$$
e^{-i\gamma H_C}
$$

where $\gamma$ is a variational parameter.

The cost unitary applies a phase to each computational basis state according to its objective value. This encodes the optimization problem into the quantum state.

---

## Mixer Hamiltonian

The **mixer Hamiltonian $H_B$** is used to explore different configurations in the solution space.

A commonly used mixer is:

$$
H_B = \sum_{i=1}^{n} X_i
$$

where $X_i$ is the Pauli-X operator acting on qubit $i$.

The corresponding mixer unitary is:

$$
U_B(\beta)
$$

$$
e^{-i\beta H_B}
$$

where $\beta$ is a variational parameter.

The mixer causes transitions between computational basis states, allowing the quantum state to explore different candidate solutions.

---

## QAOA Circuit

QAOA alternates between the cost and mixer operations.

For a single QAOA layer:

$$
|\psi(\gamma,\beta)\rangle
$$

$$
U_B(\beta)U_C(\gamma)
|+\rangle^{\otimes n}
$$

For multiple layers, the cost and mixer operations are repeated:

$$
|\psi_p\rangle =
U_B(\beta_p)U_C(\gamma_p)
\cdots
U_B(\beta_1)U_C(\gamma_1)
|+\rangle^{\otimes n}
$$

The parameters are:

$$
\{\gamma_1,\gamma_2,\ldots,\gamma_p,
\beta_1,\beta_2,\ldots,\beta_p\}
$$

The number of alternating cost-mixer pairs is called the **QAOA depth**, represented by $p$.

A larger value of $p$ gives the circuit more parameters and greater flexibility in representing the desired quantum state, but also increases circuit depth and computational requirements.

---

## Parameter Optimization

The parameters $\gamma$ and $\beta$ are optimized using a classical optimization algorithm.

For a particular set of parameters, the QAOA circuit is executed and measured. The measurement results are used to calculate the objective value.

The classical optimizer then uses this information to update the parameters and produces a new set of values.

This process is repeated until the parameters converge to a suitable solution.

The expectation value of the cost Hamiltonian is:

$$
\langle H_C\rangle
$$

$$
\langle\psi_p|
H_C
|\psi_p\rangle
$$

The parameters are optimized to maximize or minimize this expectation value depending on how the optimization problem is formulated.

---

## Hybrid Quantum-Classical Loop

The complete QAOA process can be represented as:

```text
Initialize parameters
        ↓
Prepare |+⟩⊗n
        ↓
Apply cost unitary
        ↓
Apply mixer unitary
        ↓
Measure quantum state
        ↓
Calculate objective value
        ↓
Classical optimizer updates parameters
        ↓
Repeat
        ↓
Obtain candidate solutions
```

The quantum circuit performs the quantum part of the computation, while the classical optimizer controls the variational parameters.

This interaction continues until a suitable set of parameters is obtained.

---

## Measurement

After applying the QAOA circuit, the quantum state is measured in the computational basis.

The measurement produces **bitstrings**, where each bitstring represents a possible solution to the optimization problem.

Because quantum measurement is probabilistic, the circuit is generally executed multiple times. Each execution is called a **shot**.

The measurements therefore produce a probability distribution over possible solutions.

The measured solutions can then be evaluated using the original objective function.

The objective is for high-quality solutions to have a high probability of being measured.

---

## Approximate Nature of QAOA

QAOA is an **approximation algorithm** because a finite-depth QAOA circuit does not necessarily produce the optimal solution with certainty.

The quality of the obtained solution depends on several factors:

- **QAOA depth $p$**
- **Choice of variational parameters**
- **Classical optimization method**
- **Number of measurement shots**
- **Structure of the optimization problem**
- **Noise and errors when using real quantum hardware**

Increasing $p$ gives QAOA greater expressive power, but also results in deeper quantum circuits and greater resource requirements.

---

## QAOA as a Variational Quantum Algorithm

QAOA is a type of **Variational Quantum Algorithm (VQA)**.

Variational quantum algorithms use a parameterized quantum circuit together with a classical optimization procedure.

QAOA applies this variational approach specifically to combinatorial optimization problems using alternating cost and mixer Hamiltonians.

The general variational process is:

$$
\boxed{
\text{Parameterized Quantum Circuit}
\rightarrow
\text{Measurement}
\rightarrow
\text{Objective Evaluation}
\rightarrow
\text{Classical Optimization}
\rightarrow
\text{Updated Parameters}
}
$$

---

## Optimization Problems

QAOA can be formulated for a variety of combinatorial optimization problems, including:

- **Max-Cut**
- **Max-SAT**
- **Travelling Salesman Problem (TSP)**
- **Graph optimization**
- **Scheduling**
- **Resource allocation**
- **Number partitioning**
- **Constraint optimization**

The formulation of the **cost Hamiltonian**, the **mixer**, and the encoding of the solution depend on the particular optimization problem.

---

## Key Components of QAOA

| Component | Purpose |
|---|---|
| **Qubits** | Represent the variables of the optimization problem |
| **Initial state** | Creates a superposition of possible configurations |
| **Cost Hamiltonian $H_C$** | Encodes the objective function |
| **Cost unitary $U_C(\gamma)$** | Applies the cost-dependent phase |
| **Mixer Hamiltonian $H_B$** | Enables exploration of different configurations |
| **Mixer unitary $U_B(\beta)$** | Applies the mixing operation |
| **Parameters $\gamma,\beta$** | Control the cost and mixer operations |
| **Classical optimizer** | Updates the variational parameters |
| **Measurement** | Produces candidate solutions |
| **Shots** | Provide the probability distribution of measured solutions |

---

## Summary

QAOA uses a parameterized quantum circuit to explore the solution space of a combinatorial optimization problem.

The optimization problem is encoded in a **cost Hamiltonian**, while a **mixer Hamiltonian** allows the quantum state to explore different configurations. These operations are alternated for a chosen number of layers, and their parameters are optimized using a classical optimizer.

The fundamental structure is:

$$
\boxed{
|+\rangle^{\otimes n}
\rightarrow
U_C(\gamma)
\rightarrow
U_B(\beta)
\rightarrow
\text{Measurement}
\rightarrow
\text{Classical Optimization}
}
$$

The process is repeated until suitable parameters are found, after which the measured bitstrings are evaluated to obtain high-quality candidate solutions.
