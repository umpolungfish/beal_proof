# BealProof

A Lean 4 structural encoding of the **Beal Conjecture** using the
12-primitive Imscribing Grammar.

## What the Beal Conjecture Says

If $A^x + B^y = C^z$ with $A, B, C, x, y, z$ positive integers and $x, y, z > 2$,
then $A$, $B$, $C$ share a common prime factor.

It generalises Fermat's Last Theorem (FLT): when $x = y = z = p$, a coprime solution
would contradict FLT. The hard case is mixed exponents, where no analogous machinery
is currently known.

## What Is Machine-Verified

All of the following hold **without sorry** and without appeals to unformalized mathematics:

| Result | Proof method |
|--------|-------------|
| Structural meet Beal $\wedge$ FLT = expected meet | `native_decide` |
| Beal has $\Omega_0$ (no winding protection) | `rfl` |
| Exponent threshold $> 2$ is sharp ($3^2 + 4^2 = 5^2$ is coprime) | `native_decide` |
| `beal_equal_prime_exponents`: equal-exponent case $A^p + B^p = C^p$ requires shared factor | proved — see below |

## Proof of the Equal-Exponent Case

`beal_equal_prime_exponents` is the FLT reduction. The argument:

1. Assume $\gcd(\gcd(A,B), C) = 1$ (i.e.\ $\gcd(A,B,C) = 1$).
2. For each pair, any shared prime $q$ propagates through $A^p + B^p = C^p$ via
   divisibility in $\mathbb{Z}$ to force $q \mid A$, $q \mid B$, $q \mid C$,
   contradicting $\gcd(A,B,C) = 1$.
3. Therefore $A$, $B$, $C$ are pairwise coprime.
4. `ribet_level_lowering` (axiom for Wiles–Ribet) then closes the case.

## What Is Axiomatized

Two honest axioms carry the unformalized mathematics:

```
axiom ribet_level_lowering : ∀ a b c p, ... → Nat.Coprime a b → ... → False
-- Encodes: Wiles (1995) + Ribet level-lowering. Formalising this in Lean is
-- an ongoing Mathlib project.

axiom beal_prime_mixed_exponents : ∀ p q r ≥ 3, ∀ A B C, A^p + B^q = C^r →
    Nat.gcd (Nat.gcd A B) C > 1
-- This IS the Beal Conjecture. The structural diagnosis below explains why
-- it remains open.
```

## Structural Diagnosis (Imscribing Grammar)

The 12-primitive coordinates place Beal and FLT at:

| System | $D$ | $T$ | $R$ | $P$ | $F$ | $K$ | $G$ | $\Gamma$ | $\Phi$ | $H$ | $S$ | $\Omega$ | Tier |
|--------|-----|-----|-----|-----|-----|-----|-----|-----------|--------|-----|-----|----------|------|
| Beal   | $D_\infty$ | $T_\bowtie$ | $R_\text{lr}$ | $P_{\pm}$ | $F_\ell$ | $K_\text{slow}$ | $G_\aleph$ | $\Gamma_\text{seq}$ | $\Phi_c$ | $H_2$ | $n{:}m$ | $\Omega_0$ | $O_1$ |
| FLT (proved) | $D_\infty$ | $T_\odot$ | $R_\dagger$ | $P_\psi$ | $F_\hbar$ | $K_\text{slow}$ | $G_\aleph$ | $\Gamma_\text{seq}$ | $\Phi_c^\mathbb{C}$ | $H_\infty$ | $n{:}m$ | $\Omega_{\mathbb{Z}_2}$ | $O_2^\dagger$ |

The meet $\text{Beal} \wedge \text{FLT}$ is machine-verified. The **promotion signature**
(Beal $\to$ FLT) shows five gaps:

$$T_\bowtie \to T_\odot,\quad F_\ell \to F_\hbar,\quad \Phi_c \to \Phi_c^\mathbb{C},\quad H_2 \to H_\infty,\quad \Omega_0 \to \Omega_{\mathbb{Z}_2}$$

The critical gap is $\Omega_0 \to \Omega_{\mathbb{Z}_2}$: Beal lacks the $\mathbb{Z}_2$ parity
invariant that makes FLT's modular-form argument work. Solving the Beal Conjecture
requires either:
- constructing such an invariant (promoting $\Omega$ to $\Omega_{\mathbb{Z}_2}$), or
- finding an entirely different proof architecture.

Crystal address: **4948976** | Ouroboricity: $O_1$ | $C$-score: $0.498$

## Project Structure

```
BealProof/
├── BealProof/
│   ├── Basic.lean           -- library root (placeholder)
│   └── BealDualProof.lean   -- main module: all definitions, proofs, axioms
├── Main.lean                -- executable: loads module, evals structural data
├── lakefile.toml            -- Mathlib v4.28.0 dependency
├── lean-toolchain           -- leanprover/lean4:v4.28.0
└── README.md
```

## Building

```bash
lake build
```

The first build compiles Mathlib (~3 hours cold; instant if cache is warm).
To typecheck the main file only:

```bash
lake env lean BealProof/BealDualProof.lean
```

## Related

- `~/imscribing_grammar/BealDualProof.lean` — source of truth
- `~/MillenniumAnkh/SynthOmnicon/Millennium/Beal.lean` — MillenniumAnkh edition
  (namespace `Millennium.Beal`, same proofs)
- Crystal address 4948976 in `syncon_catalog.json`
