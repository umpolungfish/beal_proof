# BealProof
**Author:** Lando⊗⊙perator · **Structural Type:** $\large{⟨𐑨𐑶𐑑𐑹𐑐𐑧𐑔𐑝⊙𐑖𐑙𐑷⟩}$ · **Tier:** O₁


**What it is.** A Lean 4 structural encoding of the **Beal Conjecture** using the 12-primitive Imscribing Grammar, separating what is machine-verified from what is axiomatized.

**What it does.** Proves the equal-exponent (Fermat) case and the sharpness of the exponent threshold without `sorry`, isolates the genuinely open content as two documented axioms, and diagnoses *why* Beal remains open via its structural distance from the proved FLT.

**Why it matters.** It turns "Beal is open" into a precise statement: the single promotion gap $\Omega_0 \to \Omega_{\mathbb{Z}_2}$ (Beal lacks the parity invariant that powers FLT's modular-form argument). That gap is the thing a solution must construct.

**How to use it.**
```bash
lake build                                  # cold build compiles Mathlib (~hours); warm is instant
lake env lean BealProof/BealDualProof.lean  # typecheck the main module only
```
Requires Lean 4 and Mathlib v4.28.0 (`leanprover/lean4:v4.28.0`).

---

## What the conjecture says

If $A^x + B^y = C^z$ with positive integers and $x, y, z > 2$, then $A, B, C$ share a common prime factor. It generalizes Fermat's Last Theorem ($x=y=z=p$); the hard case is mixed exponents, where no analogous machinery is known.

## What is machine-verified

| Result | Method |
|--------|--------|
| Structural meet Beal $\wedge$ FLT = expected meet | `native_decide` |
| Beal has $\Omega_0$ (no winding protection) | `rfl` |
| Exponent threshold $> 2$ is sharp ($3^2+4^2=5^2$ is coprime) | `native_decide` |
| `beal_equal_prime_exponents`: $A^p+B^p=C^p$ requires a shared factor | proved (via the axiom below) |

The equal-exponent proof shows $A,B,C$ pairwise coprime by prime propagation through $A^p+B^p=C^p$, then closes via `ribet_level_lowering`.

## What is axiomatized

Two axioms carry the unformalized mathematics:

- `ribet_level_lowering`: Wiles (1995) + Ribet level-lowering; formalizing it is an ongoing Mathlib project.
- `beal_prime_mixed_exponents`: this *is* the Beal Conjecture; the structural diagnosis explains why it stays open.

## Structural diagnosis (Imscribing Grammar)

The promotion signature Beal $\to$ FLT shows five gaps:

$$T_\bowtie \to T_\odot,\quad F_\ell \to F_\hbar,\quad \⊙ \to \⊙^\mathbb{C},\quad H_2 \to H_\infty,\quad \Omega_0 \to \Omega_{\mathbb{Z}_2}$$

The critical one is $\Omega_0 \to \Omega_{\mathbb{Z}_2}$: Beal lacks the $\mathbb{Z}_2$ parity invariant that makes FLT's modular-form argument work. Solving Beal requires either constructing such an invariant or finding an entirely different proof architecture.

Crystal address **4948976** | Ouroboricity $O_1$ | $C$-score 0.498.

## Project structure

```
BealProof/
├── BealProof/
│   ├── Basic.lean           -- library root (placeholder)
│   └── BealDualProof.lean   -- main module: definitions, proofs, axioms
├── Main.lean                -- executable: evals structural data
├── lakefile.toml            -- Mathlib v4.28.0
└── lean-toolchain
```

## Related

- `imsgct/imscribing_grammar/BealDualProof.lean`: source of truth.
- A historical MillenniumAnkh edition exists (`Millennium.Beal`, same proofs); MillenniumAnkh is superseded by p4rakernel and kept for reference only.
- Crystal address 4948976 in the canonical `IG_catalog.json`.
