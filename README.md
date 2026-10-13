<!-- solve-problem:b593bd42-7c51-492d-9913-d70331045517 -->
# Polynomial-time Learning Parity with Noise

A [Tangle Solve problem](https://solve.tangle.tools/problems/b593bd42-7c51-492d-9913-d70331045517).

## What is the problem?

The Learning Parity with Noise (LPN) problem. Fix a dimension *n* ∈ ℕ and a noise rate ρ ∈ (0, ½). An adversary is given access to samples of the form (a⃗ᵢ, b⃗ᵢ) where each a⃗ᵢ ∈ {0,1}ⁿ is drawn uniformly at random, b⃗ᵢ = ⟨a⃗ᵢ, s⃗⟩ ⊕ eᵢ for a fixed unknown secret s⃗ ∈ {0,1}ⁿ, and each noise bit eᵢ is 1 independently with probability ρ. The task: recover s⃗ using poly(n) samples in poly(n, 1/(½−ρ)) time. (Decision version: distinguish such samples from uniformly random pairs.) The standard parameter regime is constant ρ (classically ρ = 1/4 or, in cryptographic usage, ρ constant with n the security parameter). Note: with ρ constant, information-theoretically Θ(n) samples suffice for unique recovery; the question is purely computational. The search and decision versions are known to be polynomially equivalent (via so-called secret-recovery to distinguishing reductions), so either version resolves the problem.

## Why does it matters?

LPN is one of the simplest possible noisy-learning problems, so a poly-time algorithm would be shocking: it would collapse a large family of lightweight, post-quantum cryptographic constructions (HB/HB⁺ authentication protocols, Alekhnovich's public-key schemes, etc.) and resolve the long-standing question of whether the Statistical Query (SQ) lower bound of Kearns and later Blum–Kalai–Wasserman is an artifact of the SQ model rather than true computational hardness. Conversely, a proof of hardness (even average-case, for decision LPN) would place much of modern cryptography on firmer foundations. LPN also exactly captures decoding random linear codes over F₂, so it connects to classical coding-theoretic open problems.

## What's already known?

- **Blum, Kalai, Wasserman (2003):** the breakthrough 2^{O(n/log log n)}-time, poly(n)-sample algorithm — subexponential but far from polynomial. It works by finding many samples sharing a common noise pattern via a "covering" of a small subset of coordinates; bias amplification converts noisy parities to weakly correlated ones.
- **Lyubashevsky (2005):** reduced sample complexity of BKW to n^{1+ε} at the cost of 2^{O(n/log log n)} time still (verify exact tradeoff).
- **Coding-theoretic algorithms** (information-set decoding, e.g., Stern's, Steiner-tree based, BKW variants by Kirchner, Fouque) give 2^{Θ(n/ln n)}-type bounds (verify current best constant). The best known algorithms remain 2^{Θ(n/log n)}, so a super-polynomial gap persists.
- **Hardness results:** LPN is NP-hard in the worst case for ρ ≥ 1/log n or with adversarial noise (Håstad 2001, variants), and hard for statistical query algorithms at constant noise (Kearns; Blum–Kalai–Wasserman show Ω(n) SQ queries needed). But SQ lower bounds provably do not imply computational hardness.
- **Hardness assumptions:** LPN with constant ρ is a standard assumption underpinning HB⁺ (Juels–Weis), Alekhnovich cryptosystems, and others.

## What would count as solving it?

1. **Algorithmic resolution:** a randomized algorithm recovering s⃗ with, say, 2/3 probability using poly(n) samples and poly(n) time, for any fixed ρ < ½ (even just ρ = 1/4 would be a spectacular result).
2. **Hardness resolution:** a proof that no poly-time algorithm exists (under standard complexity assumptions, e.g., a reduction from a believed-hard problem to average-case LPN with constant ρ).
3. **Intermediate but publishable:** a 2^{o(n/log log n)} or even 2^{o(n/ log n)}-time algorithm, or a poly-time algorithm for some nontrivial sub-constant noise regime ρ(n) → ½ but ρ(n) = ½ − 1/polylog(n), or evidence (oracle separations, fine-grained reductions) that 2^{Θ(n/log n)} is optimal.

## How to start

1. Read Blum–Kalai–Wasserman (2003) thoroughly — Sections on the covering construction and bias amplification. Understand exactly where the log log n losses come from; any poly-time attack likely replaces the "birthday-paradox-like" search for equal-noise samples with something combinatorially structured.
2. Read Lyubashevsky's sample-complexity reduction and the coding-theory connection: LPN search = decoding a random linear code with rate approaching ρ; compare against best information-set-decoding bounds (May–Meuer–Thomé line of work) (verify).
3. Natural approach: seek a structured family of test vectors a⃗ such that many sample pairs agree on a subset of coordinates, generalizing BKW's "blocks" — or import techniques from lattice-based/SIS algorithms, since LWE is the module analogue. Main obstacle: the birthday/bias-amplification structure of every known algorithm inherently costs 2^{√n} or 2^{n/log n}.
4. Alternative: try to formalize hardness via reductions from planted clique or from problems on random graphs; average-case reductions to LPN are rare but the problem's extreme symmetry (invariance under GL(n,2)) helps.

## Key references

- A. Blum, A. Kalai, H. Wasserman, "Noise-Tolerant Learning, the Parity Problem, and the Statistical Query Model," *J. ACM* 50(4), 2003. First subexponential 2^{O(n/log log n)} algorithm for LPN and matching SQ lower bound. (Available via ACM DL / author pages.)
- V. Lyubashevsky, "The Parity Problem in the Presence of Noise, Decoding Random Linear Codes, and Cryptography," *STOC 2005*. Trades samples for time, connecting LPN to decoding random linear codes. (verify details)
- J. Katz, J. Shin, "Parallel and Concurrent Security of the HB and HB⁺ Protocols," *Eurocrypt 2006*. Establishes LPN-based authentication security and standardizes the LPN assumption in crypto. (verify)
- J. Håstad, "Some Optimal Inapproximability Results," *J. ACM* 48, 2001. Worst-case NP-hardness of noisy-parity-type problems. (verify relevance)

## Warnings

- Decision vs. search LPN: state which you attack; they are equivalent but reductions lose polynomial factors. Don't prove an "algorithm" for ρ ≥ ½ — recovery is information-theoretically impossible there.
- SQ lower bounds (Kearns, BKW) do **not** imply computational hardness; do not conflate them.
- Watch sample complexity: an algorithm using 2^{Θ(n)} samples is trivial (Gaussian elimination after noiseless subset search) and not a solution; polynomial samples are required.
- LPN with adversarial (worst-case) noise is NP-hard and known — the open problem is random noise with constant rate.
- Beware claimed "quantum" speedups: known quantum algorithms (via Grover/amplitude amplification) improve exponents but remain super-polynomial.

---
*`ALGO-000197` · unverified*

## Acceptance guidance

1. **Algorithmic resolution:** a randomized algorithm recovering s⃗ with, say, 2/3 probability using poly(n) samples and poly(n) time, for any fixed ρ < ½ (even just ρ = 1/4 would be a spectacular result).
2. **Hardness resolution:** a proof that no poly-time algorithm exists (under standard complexity assumptions, e.g., a reduction from a believed-hard problem to average-case LPN with constant ρ).
3. **Intermediate but publishable:** a 2^{o(n/log log n)} or even 2^{o(n/ log n)}-time algorithm, or a poly-time algorithm for some nontrivial sub-constant noise regime ρ(n) → ½ but ρ(n) = ½ − 1/polylog(n), or evidence (oracle separations, fine-grained reductions) that 2^{Θ(n/log n)} is optimal.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/b593bd42-7c51-492d-9913-d70331045517?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000197.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000197)
