# 🌀 Cyclotomic Hylomorphism, Spread Stream & StreamPolynumber Specification

Documents and verifies the **Cyclotomic Conjugate Hylomorphism Engine** bridging the 4-stage constructivist functorial pipeline:

$$\text{Cyclotomic Conjugate Hylo} \xrightarrow{\text{anamorphism}} \text{Spread Stream } Z_n(x) \xrightarrow{\text{carrier}} \text{StreamPolynumber} \xrightarrow{\text{Cigler-Herbig sieve}} \text{Goh Factorisation} \xrightarrow{\text{spectral decomposition}} \text{Block-Diagonal Companion Maxel}$$

---

## 1. Functorial Pipeline Foundations

1. **Deforested Cyclotomic Hylomorphism**:
   Unfolds degree $N$ into its divisor stream $d \mid N$ via `divisorStream N` in $O(1)$ auxiliary registers, lifts each divisor to its irreducible Cigler-Herbig factor $\Psi_d(x)$ as a `StreamPolynumber`, and folds them via Cauchy polynomial convolution into the scaled spread polynomial $Z_N(x) = \prod_{d \mid N} \Psi_d(x)$.

2. **Gauss Totient Sum Conservation**:
   Every degree $N$ satisfies:
   $$\sum_{d \mid N} \phi(d) = N$$
   guaranteeing that the algebraic capacity of the irreducible components saturates the manifest dimension without deficit or excess.

3. **Block-Diagonal Spectral Maxel Direct Sum**:
   The direct sum of companion matrices across all divisors $d \mid N$:
   $$\mathbf{M}(Z_N) \cong \bigoplus_{d \mid N} \mathbf{M}(\Psi_d)$$
   has exact dimension $N \times N$, acting as an uncoupled linear vector space homomorphism on $\mathbb{Z}^N$.

4. **Observer Epoch Fixed Point ($N = 39 = 3 \times 13$)**:
   The Observer Epoch partitions into divisors $\{1, 3, 13, 39\}$ with totients $1 + 2 + 12 + 24 = 39$, where $\phi(39) = 24 = Q_{\text{Green}}$ is the Green quadrance of the Pythagorean fixed point $(4, 3) \to (25, 7, 24)$.

---

## 2. Executable Idris 2 Specification Code

```idris
module Wiki.CyclotomicSpreadHyloSpec

import Stage0.BoxInt
import Stage0.Multiset
import Stage0.OnSeq.FusedStream
import Stage0.StreamPolynumber
import Stage1.Math.OnSeq.SpreadStream
import Stage1.Goh.SpreadFactorization
import Stage1.MaxelPolyBridge
import Wiki.Generators

%default total

||| Property 1 (Generative QuickCheck): Gauss Totient Sum Conservation
public export
prop_gaussTotientConservationGen : Nat -> Bool
prop_gaussTotientConservationGen nRaw =
  let n = S (clampNat nRaw 60)
  in verifyTotientConservation n

||| Property 2 (Generative QuickCheck): Lucas StreamPolynumber matches integer evaluation at x = 1
public export
prop_lucasStreamPolyGen : Nat -> Bool
prop_lucasStreamPolyGen nRaw =
  let n = S (clampNat nRaw 6)
      streamVal = evalStreamPolyNat 100 (intToBoxInt 1) (lucasStreamPoly n)
      intVal = evalLucasInteger n 1
  in streamVal == intToBoxInt intVal

||| Property 3 (Static Invariant): Observer Epoch n = 39 Certified Cyclotomic Spread
public export
prop_observerEpochSpreadStatic : Bool
prop_observerEpochSpreadStatic =
  let cs39 = certifiedCyclotomicSpread39
  in cs39.degree == 39 && cs39.totientCapacity == 39

||| Property 4 (Static Invariant): 4 Geometries Totient Conservation
public export
prop_fourGeometriesTotientStatic : Bool
prop_fourGeometriesTotientStatic =
  auditCertifiedSpreadObserverEpochProof

||| Main Verification Suite for Cyclotomic Spread Hylomorphism
public export
auditCyclotomicSpreadHyloProof : IO Bool
auditCyclotomicSpreadHyloProof = do
  let r1 = qc prop_gaussTotientConservationGen
  let r2 = qc prop_lucasStreamPolyGen
  let p3 = prop_observerEpochSpreadStatic
  let p4 = prop_fourGeometriesTotientStatic
  let p5 = auditCyclotomicStreamPolyProof
  let p6 = auditLucasStreamPolyProof
  pure (r1.pass == Just True && r2.pass == Just True && p3 && p4 && p5 && p6)
```
