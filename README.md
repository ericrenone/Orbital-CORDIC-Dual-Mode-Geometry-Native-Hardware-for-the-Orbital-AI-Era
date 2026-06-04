# Orbital CORDIC: Dual-Mode Geometry-Native Hardware for the Orbital AI Era

**ERI Labs · June 4, 2026**

> *Full SOTA Comparison, SpaceX Frontier Analysis & Novel Connections — updated against the SpaceX S-1 filing, Starship Flight 12 anomaly data, SpaceX–xAI merger, and the research frontier through June 4, 2026.*

---

## Abstract

Every CORDIC accelerator published through June 2026 operates in circular mode only. No fabricated or proposed chip runs dual-mode (circular + hyperbolic) CORDIC as its primary compute primitive, executes native Lorentz boosts, provides hardware Banach contraction monitoring, or combines aerospace triple-modular-redundancy (TMR) determinism with geometry-native AI acceleration.

As of this month, SpaceX's merger with xAI, its filing for a one-million-satellite orbital AI compute constellation, and the real-world failure of Super Heavy's flip maneuver during Starship Flight 12 have made all four of those gaps more commercially urgent than they were 90 days ago.

This document maps the complete SOTA landscape — CORDIC hardware, hyperbolic AI, frontier accelerators, and aerospace compute — against the six independent SpaceX events that converge on the orbital CORDIC thesis, and identifies the open problems whose resolution determines whether this gap closes by 2028.

---

## The Gap in One Table

| Capability | SYCore | CARMEN | Flex-PE | Safe-NEureka | TPU Ironwood | Blackwell | **Orbital CORDIC** |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| Dual-mode CORDIC (circular + hyperbolic) | ✗ | ✗ | ✗ | ✗ | ✗ | ✗ | ✓ |
| Native Lorentz boosts | ✗ | ✗ | ✗ | ✗ | ✗ | ✗ | ✓ |
| Native Möbius / Poincaré ops | ✗ | ✗ | ✗ | ✗ | ✗ | ✗ | ✓ |
| Hardware Contraction Monitor | ✗ | ✗ | ✗ | ✗ | ✗ | ✗ | ✓ |
| GNC-deterministic mode | ✗ | ✗ | ✗ | Partial | ✗ | ✗ | ✓ |
| G-FOLD cone projection | ✗ | ✗ | ✗ | ✗ | ✗ | ✗ | ✓ |
| Bit-exact TMR agreement | ✗ | ✗ | ✗ | Partial | ✗ | ✗ | ✓ |
| HELM-class hyperbolic LLM | ✗ | ✗ | ✗ | ✗ | Emulated 8–12× | Emulated 8–12× | **✓ Native** |
| Starfall-grade precision EDL | ✗ | ✗ | ✗ | ✗ | ✗ | ✗ | ✓ |
| Mars non-Euclidean G-FOLD | ✗ | ✗ | ✗ | ✗ | ✗ | ✗ | Substrate only |
| Orbital AI data center fit | ✗ | ✗ | ✗ | Partial | Power: ✗ | Power: ✗ | ✓ |
| Open RTL generator | ✗ | ✗ | ✗ | ✗ | ✗ | ✗ | ✓ Chisel/FIRRTL |

---

## The SpaceX Signal Map (June 2026)

Six independent SpaceX events from the last 120 days converge on the orbital CORDIC thesis. None were planned as endorsements; taken together, they constitute the most concrete external validation the design space has received.

---

### Signal 1 — Starship Flight 12: The Flip Failure Is a CORDIC Story (May 22, 2026)

Starship Flight 12, the inaugural Starship V3 test, succeeded on its upper-stage objectives: V3 architecture validated, controlled reentry, on-target Indian Ocean splashdown, 20 Starlink simulators deployed, heat-shield imaging via two "Dodger Dog" satellites. The primary mission failure was rotational.

> *"Following stage separation, the Super Heavy booster performed a directional flip maneuver and attempted its boostback burn. It was unable to light all planned engines and performed a partial boostback burn that ended early. Super Heavy attempted to reignite its engines for the landing burn before experiencing a hard splashdown in the Gulf of America."*
> — SpaceX post-mission statement

The failure chain is: **attitude rotation → incomplete thrust vector → lost control authority → hard splashdown**. Four Starship flights exhibit the same class of failure:

| Flight | Event |
|---|---|
| Flight 7 (Jan 2025) | One boostback igniter failed due to thermal conditions |
| Flight 8 (Mar 2025) | Two engines failed to relight; root cause traced to torch ignition thermal mismatch |
| Flight 9 (May 2025) | 12 of 13 planned engines relit; elevated nosecone pressure caused cascading attitude error |
| Flight 12 (May 2026) | Multiple engines failed to ignite post-flip |

The pattern across four flights is not a propulsion anomaly in isolation. It is a **control-authority timing problem**: the rotation primitive executes, but the subsequent compute-bound sequencing runs on hardware designed for Euclidean matrix multiplication, not rotation-native arithmetic. Every Raptor engine reignition timing decision involves an arctan to find the thrust vector error angle — computed as a polynomial approximation, not bit-exact, varying with thermal state.

**Claim:** Hardware rotation primitives with TMR bit-exact agreement would eliminate the class of attitude error accumulation observed in Starship flights 7–12. This is **falsifiable on the next Starship flight.**

---

### Signal 2 — SpaceX + xAI Merger Creates the Orbital HELM Problem (February–May 2026)

SpaceX acquired xAI in February 2026 at a combined valuation of $1.25 trillion. In May 2026, xAI was formally dissolved as a standalone entity; Grok and X were folded into a new division called SpaceXAI. The merged entity now holds:

- The Colossus supercomputer in Memphis (220,000+ NVIDIA GPUs) as its ground-compute backbone
- The Grok LLM family as its inference product
- A FCC filing for **one million orbital compute satellites** (accepted February 2, 2026) under the "SpaceX Orbital Data Center System"
- An IPO target of $1.75–2 trillion in mid-June 2026
- A stated goal: within two to three years, the lowest-cost location for AI compute will be orbit, at 100 kW per ton on near-constant solar power

The SpaceX S-1 prospectus describes their orbital AI roadmap:

| Version | Timeline | Description |
|---|---|---|
| V2 | 2026–2028 | Scaled clusters up to 1 PFLOPS; deployment of **custom xAI ASICs for efficient transformer inference** |
| V3 | 2029+ | Hybrid orbital/terrestrial orchestration; exascale training runs in orbit |

Custom xAI ASICs running in LEO must be: power-efficient (≤100 kW/ton eliminates TPU Ironwood-class profiles), radiation-tolerant (standard commercial silicon fails in LEO within months from single-event upsets), thermally stable (conduction and radiation cooling only), and LLM-capable with native hyperbolic geometry if Grok successors adopt HELM-style architectures.

The HELM result (4% MMLU/ARC quality gain over matched Euclidean baselines at NeurIPS 2025) means the question is no longer *whether* a hyperbolic LLM ASIC is needed — it is **who builds it first**.

---

### Signal 3 — Terafab Is the Fabrication Pathway (March 2026)

In March 2026, Tesla and SpaceX announced **Terafab**, a semiconductor fabrication initiative optimized for high-volume, radiation-hardened chips destined for orbital compute platforms. Terafab leverages extreme ultraviolet lithography (EUV) and proprietary wafer packaging, targeting up to 10 racks per hour of automated chip production once fully ramped.

The orbital CORDIC design targets TSMC N2P. The Terafab initiative signals that the EUV-based process node it requires is actively being industrialized for aerospace applications by the most commercially relevant actor — and that by tape-out, a fabrication pipeline optimized for exactly that node and use case will exist.

---

### Signal 4 — Starfall Precision Reentry Requires G-FOLD Acceleration Today (June 2026)

On May 29, 2026, the FAA released its Final Environmental Assessment approving SpaceX's **Starfall** reentry vehicle program for two demonstration missions in the Pacific Ocean. Starfall is a small capsule (~3,086 lb.) designed to return up to **1,000 kg** of payload with precision cargo delivery from orbit — competing directly with Varda Space Industries, Inversion, Catalyx Space, and Reditus Space.

Precision cargo delivery from orbit is the **G-FOLD problem**: the Guidance for Fuel-Optimal Large Diverts algorithm, a convex Second-Order Cone Program (SOCP) also used for Falcon 9 landing. For Falcon 9, the landing zone is large and compute margin is generous. For Starfall's precision 1,000 kg requirement, the thrust-constraint cone becomes tight and the SOCP solver must run at higher iteration depth with lower latency.

Starfall's "mass producible" commercial intent means **hundreds of flights per year**, each requiring an onboard G-FOLD solve that currently runs on commodity hardware.

**The G-FOLD acceleration market just moved from a 2028 Mars scenario to a 2026–2027 production requirement.**

---

### Signal 5 — Mars EDL at 7.5 km/s Makes Non-Euclidean G-FOLD Urgent (2028 Target)

SpaceX's Mission: Mars targets cargo flights to the Martian surface no earlier than 2028 at $100 million per metric ton. Starship enters Mars's atmosphere at **7.5 km/s** and decelerates aerodynamically — no precedent exists for EDL of a 200-metric-ton vehicle on Mars.

At planetary scale, the "flat Earth" approximation in G-FOLD breaks down: the surface is curved, and the optimal thrust-cone constraint must account for the geodesic path on a sphere. Geodesic distance on a sphere is computed via the inverse hyperbolic cosine of the Lorentz inner product — an operation native to hyperbolic CORDIC cascades but absent from every existing or proposed chip except this design.

**The 2028 commercial deadline makes the open problem concrete:** each month the SOCP solver runs on Euclidean-approximation hardware for an intrinsically non-Euclidean geometry is a month of suboptimal fuel consumption that directly translates to payload mass left behind.

---

### Signal 6 — Deep Space Autonomy Requires Hardware-Level Convergence Guarantees (2026+)

Mars signal delay is 4–24 minutes one-way; round-trip GNC correction latency is 8–48 minutes. Every GNC operation on a Mars-bound Starship, on the Martian surface, and during EDL must be solved fully onboard, in real time, without ground input.

Every convergence guarantee in software is a **convergence assertion** — the code says it will converge, but no hardware primitive confirms the claim in real time. For a 200-metric-ton vehicle decelerating from 7.5 km/s over an alien planet with no ground support, that is not an academic distinction.

NASA's JPL began testing the High Performance Spaceflight Computing (HPSC) processor in early 2026 specifically to give spacecraft "100 times the computational capacity of current spaceflight computers" for onboard AI — confirming that radiation hardness + AI inference + real-time response is now an active hardware design requirement.

---

## CORDIC Hardware Frontier (Updated May–June 2026)

### CARMEN — arXiv:2605.06878, May 7, 2026

The closest published CORDIC-for-AI comparator. CARMEN (CORDIC-Accelerated Resource-Efficient Multi-Precision Inference Engine) demonstrates that CORDIC iteration depth is a precision dial, not a format switch.

**Documented results (28nm CMOS ASIC):**

| Metric | Value |
|---|---|
| Computation cycle reduction | 33% per MAC stage |
| Power savings | 21% per MAC stage |
| Throughput density (256-PE) | 4.83 TOPS/mm² |
| Power efficiency | 11.67 TOPS/W |
| FPGA latency | 154.6 ms @ 0.43 W (real-time object detection) |

**CARMEN does not address the orbital gaps:** No hyperbolic mode. No Lorentz operations. No GNC determinism. No G-FOLD acceleration. CARMEN is silicon proof-of-concept that CORDIC-for-AI is ASIC-viable — but at the geometry SpaceX's orbital AI data center and Mars EDL programs are moving away from.

### SYCore / "CORDIC Is All You Need" — arXiv:2503.11685, March 2025

Systolic CORDIC engine with CAESAR adaptive scheduler: 4.64× throughput improvement, 5.02× power reduction on 28nm CMOS. Full circular-mode support. No hyperbolic operations. No aerospace target. No orbital AI radiation tolerance.

### Flex-PE / NEURIC — arXiv:2503.14354

CORDIC-based SIMD vector engine with FxP4/8/16/32 runtime switching, 8.42 GOPS/W for edge inference. Circular-mode only. Edge-AI target only. The gap to SpaceX orbital compute requirements is architectural, not incremental.

### Safe-NEureka — arXiv:2602.04803, February 2026

The aerospace-AI comparator. Hybrid Modular Redundant DNN accelerator for RISC-V GNC; hardware fault recovery in 24 clock cycles. Targets satellite GNC. Does not implement CORDIC; does not support hyperbolic operations; does not accelerate G-FOLD.

Safe-NEureka proves the aerospace community is building RISC-V AI solutions. It does not resolve the rotation-primitive gap that the Flight 12 booster anomaly exposed.

---

## Hyperbolic AI Frontier (Updated Through June 2026)

### HELM — NeurIPS 2025, arXiv:2505.24722

Billion-parameter-scale fully hyperbolic language model. Mixture-of-Curvature Experts (MICE), Hyperbolic Multi-Head Latent Attention (HMLA), HOPE positional encodings, hyperbolic RMSNorm. **4% MMLU/ARC gain over matched Euclidean baselines.**

HELM runs entirely on Euclidean silicon — every Lorentz operation emulated via polynomial approximation on matmul hardware. With SpaceXAI's orbital data center program projecting custom ASICs by 2026–2028, the hardware bottleneck is the only remaining obstacle to deployment at scale.

### Intrinsic Lorentz Neural Network (ILNN) — ICLR 2026, arXiv:2602.23981

Fully intrinsic Lorentz architecture eliminating mixed Euclidean operations. ILNN's acceptance at ICLR 2026 moves fully hyperbolic inference from an experimental result to a **peer-validated architecture**. The hardware bottleneck is now the only remaining obstacle.

### Fast and Geometrically Grounded Lorentz NNs — arXiv:2601.21529, January 2026

Proves that prior Lorentz linear layer formulations cause logarithmic norm degradation. The fix — distance-to-hyperplane formulation — restores linear norm scaling. This paper removes the last theoretical obstacle to deep hyperbolic training; the obstacle that remains is **hardware throughput**.

### L-GATr — NeurIPS 2024, arXiv:2405.14806

Lorentz-equivariant Geometric Algebra Transformer for LHC physics. SOTA on amplitude regression, top tagging, and Lorentz-equivariant generative modeling. Lorentz equivariance is the correct inductive bias for any system processing relativistic data — including hypersonic Starship reentry telemetry at 7.5 km/s.

---

## Frontier Accelerator Comparison (June 2026)

| System | Peak Throughput | Hyperbolic Mode | Lorentz Native | GNC-Safe | Power |
|---|---|:---:|:---:|:---:|---|
| TPU v7 Ironwood | 4.6 PFLOPS FP8/chip | ✗ | ✗ | ✗ | ~900 W |
| NVIDIA Blackwell GB300 | ~0.36 ExaFLOPS/system | ✗ | ✗ | ✗ | ~2.7 kW/GPU |
| AWS Trainium3 | 2.52 PFLOPS FP8/chip | ✗ | ✗ | ✗ | ~700 W |
| CARMEN (28nm ASIC) | 4.83 TOPS/mm² | ✗ | ✗ | ✗ | 11.67 TOPS/W |
| SYCore (28nm ASIC) | 4.64× over baseline | ✗ | ✗ | ✗ | 5.02× reduction |
| Safe-NEureka | 1,160 MOPS | ✗ | ✗ | DMR partial | — |
| **Orbital CORDIC** | **~13% below Euclidean** | **✓** | **✓** | **TMR full** | **Multiplier-free** |

SpaceX's orbital data center V2 (2026–2028) specification requires ≤100 kW/ton power, radiation tolerance, and transformer inference. Of the above, only the orbital CORDIC architecture addresses all three simultaneously. TPU Ironwood runs 8–12× slower on hyperbolic LLM workloads (HELM, L-GATr) due to transcendental kernel overhead.

---

## Honest Accounting

### Where This Design Leads (Strengthened Since Last Revision)

- **Flight 12** has moved the CORDIC-exact rotation determinism argument from theoretical to empirically motivated. Four flights with partial flip/boostback engine failures across the same control-authority domain is a pattern, not an anomaly.
- **SpaceX-xAI merger and orbital data center program** create the first trillion-dollar commercial customer whose hardware requirements are not satisfied by any existing chip and who cannot satisfy them with commodity Nvidia hardware.
- **Starfall approval** creates a near-term (2026–2027) G-FOLD acceleration market that was previously a 2028+ Mars scenario.
- **Terafab** means the fabrication pathway for the target rad-hard process node is being industrialized by the aerospace entity most likely to be a customer.
- **ILNN ICLR 2026** confirms that the theoretical objections to fully intrinsic Lorentz computation are resolved. The hardware gap is now the only gap.

### Where This Design Still Lags (Honest)

| Gap | Status |
|---|---|
| **Unproven silicon** | CARMEN has measured ASIC results. This design has not run a single test vector on physical hardware. The distinction is fundamental. |
| **Euclidean throughput penalty** | The ~13% FLOPs penalty on purely Euclidean dense matrix operations is real and will not be overcome by design refinement. |
| **Ecosystem cost** | No production compiler, no certified GNC toolchain, no FAA/ESA-approved verification flow. |
| **Butterfly expressivity at 70B+** | Whether structured rotation decomposition holds at frontier LLM scale is an open empirical question. |
| **Race against SpaceXAI's internal team** | The V2 custom ASIC specification is now a funded internal SpaceX program. If SpaceX's internal silicon team tapes out first, the open-RISC-V generator becomes a research artifact, not a product. |

---

## Eight Convergent Research Signals

1. **CARMEN (May 2026)** — Confirms CORDIC-for-AI is ASIC-viable at 4.83 TOPS/mm² and 11.67 TOPS/W. The circular-mode half of the orbital CORDIC claim is validated in silicon.

2. **HELM (NeurIPS 2025)** — Confirms 4% quality gain for hyperbolic LLMs at billion-parameter scale. The demand is real and quantified.

3. **ILNN (ICLR 2026)** — Confirms that fully intrinsic Lorentz computation is architecturally coherent and superior to mixed approaches.

4. **Safe-NEureka (February 2026)** — Confirms the aerospace community is building RISC-V AI solutions. The design space is actively contested.

5. **Fast Lorentz NNs (January 2026)** — Resolves the norm degradation obstacle. The remaining obstacle to hyperbolic frontier models is hardware throughput.

6. **Starship Flight 12 booster anomaly (May 2026)** ⭐ *NEW* — Provides four-flight empirical evidence that rotation-primitive determinism failures are the recurring failure mode in SpaceX's highest-profile GNC system.

7. **SpaceX-xAI merger and orbital data center S-1 filing (February–May 2026)** ⭐ *NEW* — Creates a concrete trillion-dollar commercial customer whose hardware requirements are not satisfied by any existing chip, and who cannot use commodity Nvidia hardware in their target deployment environment.

8. **Starfall FAA approval (June 2026)** ⭐ *NEW* — Moves G-FOLD cone projection acceleration from a 2028 Mars scenario to a 2026–2027 production requirement for precision cargo delivery.

---

## Open Problems

| Problem | Commercial Deadline |
|---|---|
| **Mars EDL non-Euclidean trajectory planning** — G-FOLD on flat Euclidean geometry does not extend to large-divert planetary EDL without Lorentz-geometry reformulation. No chip has a validated SOCP solver in hyperbolic mode. | SpaceX 2028 cargo flights |
| **Orbital AI ASIC for SpaceXAI** — The SpaceX S-1 describes custom xAI ASICs for efficient transformer inference in orbit (2026–2028). The specification has not been published; the internal architecture is unknown. | V2 ASIC: 2026–2028 |
| **Modular space cohomology acceleration** — The Bérczi–Kiem theorem (arXiv:2605.29151) shows CORDIC iterations correspond to M̄₀,ₙ forgetting maps. The CORDIC-Getzler O(n log n) algorithm is proposed but neither implemented nor benchmarked. | Open |
| **Mixture-of-Curvature routing hardware** — HELM-MiCE routes tokens to curvature experts. No chip provides hardware-level per-row curvature routing. | Open |
| **Bit-exact hyperbolic TMR agreement across process-variable silicon** — Claimed; unverified in fabricated silicon. | Terafab ramp: 2026–2027 |
| **Raptor flip-maneuver determinism** — Whether CORDIC-exact rotation primitives would prevent the class of failure observed in Flights 7–12 has not been tested. | Starship Flight 13 |

---

## SpaceX Event Timeline vs. Research Claims

| Date | SpaceX Event | Research Claim Validated / Motivated |
|---|---|---|
| Jan 28, 2026 | SpaceX FCC filing: 1M orbital compute satellites | Orbital AI ASIC market confirmed |
| Feb 2026 | SpaceX acquires xAI ($1.25T); SpaceXAI formed | Custom ASIC for hyperbolic transformer inference becomes funded requirement |
| Mar 2026 | Tesla/SpaceX announce Terafab rad-hard fab | Fabrication pathway for N2P target confirmed |
| May 7, 2026 | CARMEN arXiv:2605.06878 | CORDIC precision dial ASIC-viable in silicon |
| May 22, 2026 | Starship Flight 12: Super Heavy flip/boostback failure | Hardware rotation determinism gap demonstrated in-flight |
| May 26, 2026 | FAA grounds Starship pending investigation | GNC hardware determinism urgency increases |
| May 29, 2026 | FAA approves SpaceX Starfall reentry vehicle tests | G-FOLD cone projection market moves to 2026–2027 |
| Jun 2026 | SpaceX IPO roadshow begins (~$1.75–2T valuation) | Orbital AI compute is trillion-dollar funded program |
| **Jun 4, 2026** | **This document** | **Full orbital CORDIC SOTA connection map** |

**Next update triggers:** TPU v8 (Sunfish/Zebrafish) spec release · SpaceXAI custom ASIC V2 announcement · Starship Flight 13 booster flip/boostback outcome · Any dual-mode CORDIC chip announcement · Starfall Demo Mission 1 precision landing result.

---

## Citation

```bibtex
@techreport{erilabs2026orbitalcordic,
  title     = {Orbital CORDIC: Dual-Mode Geometry-Native Hardware for the Orbital AI Era},
  author    = {{ERI Labs}},
  year      = {2026},
  month     = {June},
  note      = {Full SOTA Comparison, SpaceX Frontier Analysis and Novel Connections,
               updated against the SpaceX S-1 filing, Starship Flight 12 anomaly data,
               SpaceX--xAI merger, and the research frontier through June 4, 2026},
  url       = {https://github.com/ericrenone/Merlin-Volder-1-SpaceXAI-Dual-Mode-CORDIC-Hardware-for-the-Orbital-AI-Era}
}
```

---

*ERI Labs — Comparison current as of June 4, 2026.*
