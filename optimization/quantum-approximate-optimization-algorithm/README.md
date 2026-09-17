# Quantum Approximate Optimization Algorithm (QAOA)

The **Quantum Approximate Optimization Algorithm (QAOA)** is a **hybrid quantum-classical algorithm** designed to find approximate solutions to **combinatorial optimization problems**. It was introduced by Edward Farhi, Jeffrey Goldstone, and Sam Gutmann in 2014.

QAOA combines a **parameterized quantum circuit** with a **classical optimization algorithm**. The quantum circuit generates a probability distribution over possible solutions, while the classical optimizer adjusts the parameters of the circuit to increase the probability of obtaining high-quality solutions.

---

## Core Idea

Many combinatorial optimization problems involve finding the best solution from a large set of possible configurations.

QAOA encodes these possible configurations into the computational basis states of a quantum system. It then uses two alternating quantum operations:

- **Cost Hamiltonian** — represents the objective function of the optimization problem.
- **Mixer Hamiltonian** — allows the quantum state to explore different possible configurations.

The parameters controlling these operations are optimized using a classical computer.

The general idea is:

> **Encode the optimization problem → explore solutions using a quantum circuit → evaluate the solutions → optimize the circuit parameters classically.**

---

## Initial Quantum State

For a problem involving \(n\) binary variables, QAOA uses \(n\) qubits.

The qubits are initialized in the equal superposition state:

$$
|+\rangle^{\otimes n}
$$

where

$$
|+\rangle =
\frac{|0\rangle + |1\rangle}{\sqrt{2}}
$$

This produces a superposition of all computational basis states:

$$
|+\rangle^{\otimes n}
=
\frac{1}{\sqrt{2^n}}
\sum_{x\in\{0,1\}^n}|x\rangle
$$

where each \(x\) represents a possible solution to the optimization problem.

---

## Cost Hamiltonian

The **cost Hamiltonian \(H_C\)** encodes the objective function of the optimization problem.

Each computational basis state \(|x\rangle\) corresponds to a possible solution, and the Hamiltonian assigns it a cost:

$$
H_C|x\rangle = C(x)|x\rangle
$$

where \(C(x)\) represents the objective or cost associated with solution \(x\).

The corresponding cost unitary is:

$$
U_C(\gamma)
=
e^{-i\gamma H_C}
$$

where \(\gamma\) is a variational parameter.

The cost layer modifies the **phase** of each state according to its objective value, allowing the optimization problem to influence the quantum state.

---

## Mixer Hamiltonian

The **mixer Hamiltonian \(H_B\)** is used to explore different configurations in the solution space.

A commonly used mixer is:

$$
H_B = \sum_{i=1}^{n}X_i
$$

where \(X_i\) is the Pauli-X operator acting on qubit \(i\).

The corresponding mixer unitary is:

$$
U_B(\beta)
=
e^{-i\beta H_B}
$$

where \(\beta\) is a variational parameter.

The mixer allows the quantum state to move between different computational basis states and explore alternative candidate solutions.

---

## QAOA Layers

QAOA alternates between the cost and mixer operations.

For a single QAOA layer:

$$
|\psi(\gamma,\beta)\rangle
=
U_B(\beta)U_C(\gamma)
|+\rangle^{\otimes n}
$$

For \(p\) layers:

$$
|\psi_p\rangle =
U_B(\beta_p)U_C(\gamma_p)
\cdots
U_B(\beta_1)U_C(\gamma_1)
|+\rangle^{\otimes n}
$$

The parameter set is therefore:

$$
\{\gamma_1,\ldots,\gamma_p,
\beta_1,\ldots,\beta_p\}
$$

The value of \(p\) is known as the **QAOA depth**.

A larger \(p\) provides more variational parameters and allows the circuit to represent more complex quantum states, but it also results in a deeper circuit and greater computational requirements.

---

## Parameter Optimization

The parameters \(\gamma\) and \(\beta\) are determined through a **classical optimization loop**.

For a particular set of parameters, the quantum circuit is executed and the resulting state is measured. The measurements are used to calculate the objective value. A classical optimizer then updates the parameters, and the process is repeated.

The objective is generally expressed through the expectation value:

$$
\langle H_C\rangle
=
\langle\psi_p|
H_C
|\psi_p\rangle
$$

The parameters are optimized to maximize or minimize this expectation value, depending on how the optimization problem is formulated.

---

## Hybrid Quantum-Classical Process

The complete QAOA procedure can be summarized as:

```text
Initialize QAOA parameters
          ↓
Prepare |+⟩⊗n
          ↓
Apply cost unitary
          ↓
Apply mixer unitary
          ↓
Measure the quantum state
          ↓
Evaluate the objective function
          ↓
Classical optimizer updates parameters
          ↓
Repeat until convergence
          ↓
Obtain high-quality candidate solutions
