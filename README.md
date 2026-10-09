# Warm-Start QAOA

Warm-start QAOA initialises the quantum circuit from the solution of a classical relaxation of the optimization problem, rather than from the uniform superposition. This lets QAOA inherit the performance guarantees of that classical relaxation and helps reduce circuit depth.

This repository contains the demo notebook from **Executing warm-start QAOA on superconducting qubits**, presented by Daniel J. Egger and Ibrahim Shehzad of IBM Quantum as the closing session of the qBraid Community Webinar Series on 7 October 2026.

The notebook walks the full quantum optimization stack on a MaxCut problem: first a four-qubit example on a simulator, then the same workflow scaled up and executed on IBM hardware.

## What the notebook covers

- **Problem modeling** — building a MaxCut instance and mapping it to an Ising Hamiltonian with the Qiskit Optimization Mapper addon
- **Continuous relaxation** — dropping the binary constraint to get a convex QP, and solving it classically
- **Circuit creation** — standard QAOA against warm-start QAOA, including the custom mixer `RY(θᵢ) RZ(−2β) RY(−θᵢ)` whose ground state is the warm-start state
- **Regularization** — projecting the relaxed solution into `[ε, 1−ε]` so qubits at the poles still mix
- **Execution** — running on a simulator, then on hardware via Qiskit Runtime
- **Post-processing** — comparing cut values against the known optimum

Most code cells ship with their saved output, so you can read the full argument and see the real hardware results without running anything.

## Launch on qBraid

Use the **Launch on qBraid** button on this tutorial's page in the qBraid Explore hub. It clones the repository into qBraid Lab. Select a Qiskit v2 environment as the notebook kernel — the packages in `requirements.txt` are already there. For selecting environments and kernels, see the [environments guide](https://docs.qbraid.com/lab/user-guide/environments).

## Running locally

```bash
pip install -r requirements.txt
jupyter lab warm-start-qaoa.ipynb
```

The hardware section needs an IBM Quantum account and a Qiskit Runtime instance. The Open (free) plan is enough. If you would rather not spend QPU time, cell 43 reads the trained parameters from `ws_qaoa_backup_hardware_parameters.txt` so you can reproduce the downstream analysis directly.

## Papers

- D. J. Egger, J. Mareček and S. Woerner, *Warm-starting quantum optimization*, Quantum **5**, 479 (2021) — [arXiv:2009.10095](https://arxiv.org/abs/2009.10095)
- E. Farhi, J. Goldstone and S. Gutmann, *A quantum approximate optimization algorithm* — [arXiv:1411.4028](https://arxiv.org/abs/1411.4028)
- N. Mohseni, J. P. Houle, I. Shehzad, G. Cortiana, C. O'Meara and A. B. Watts, *Constrained quantum optimization at utility scale: application to the knapsack problem* (2026) — [arXiv:2603.00260](https://arxiv.org/abs/2603.00260)

Since the original warm-start paper, the line of work has continued: SDP-initialized warm starts ([arXiv:2010.14021](https://arxiv.org/abs/2010.14021)), custom mixers ([arXiv:2112.11354](https://arxiv.org/abs/2112.11354)), approximation-ratio bounds at `p=1` ([arXiv:2402.12631](https://arxiv.org/abs/2402.12631)), and known failure modes such as getting stuck from a single bitstring ([arXiv:2207.05089](https://arxiv.org/abs/2207.05089)) and slow convergence near the poles ([arXiv:2410.00027](https://arxiv.org/abs/2410.00027)).

## Tooling

- [qiskit-addon-opt-mapper](https://github.com/Qiskit/qiskit-addon-opt-mapper) — builds cost operators from an abstract problem formulation. Supports continuous, integer, binary and spin variables, terms of arbitrary order, and both equality and inequality constraints. Pre-1.0, so expect the API to move.
- [qaoa_training_pipeline](https://github.com/qiskit-community/qaoa_training_pipeline) — layer-by-layer classical training of QAOA parameters.

## Watch the talk

- [Recording on YouTube](https://youtu.be/9yuaGexDEDA) — Daniel covers the theory from 0:00, Ibrahim's live demo of this notebook starts at 21:40
- [Slides (PDF)](https://qbraid-community-media.s3.amazonaws.com/marketing-site/ibm-warm-start-qaoa-slides.pdf)

## Credits

Notebook and demo by **Ibrahim Shehzad** (Quantum Algorithm Engineering, IBM Quantum). Accompanying talk by **Daniel J. Egger** (Senior Research Scientist, IBM Research Zurich). Published here with permission as part of the qBraid Community Webinar Series.

## Run quantum on qBraid

qBraid gives you a browser-based environment with Qiskit, Cirq, Braket and more preinstalled, plus access to real quantum hardware — no local setup.

Webinar viewers get **50% off** subscriptions and credit purchases with code `QCOMMUNITY50` at [account.qbraid.com](https://account.qbraid.com).

## License

Apache License 2.0. See [LICENSE](LICENSE).
