# Help & Guide: Moon-Stone Semantic Vault

Welcome to the **Moon-Stone Semantic Vault** interface (`moon-stone-vault.glyphai.ie`). This platform implements the **GlyphAI** framework—a system for semantic-preserving text transformation with codebook-controlled non-interpretability. The following guide quantifies the interface functionality for visitors, security auditors, researchers, and developers, adhering strictly to the formalisms established in Chapters 1–15 and Appendix E of the monograph by Vitali de Mille (2026).

---

## 1. Architectural Foundations & Tier Hierarchy

This interface visualizes the operation of the **Tier 3 Model-as-Key Bound** (Chapter 5). The data displayed here ($g$) does not represent natural language text, but rather *semantic glyphs*. Their interpretation is computationally impossible without the authorized domain context ($C_{dom}$), model ($M$), and policy vector ($\pi$).

* **Irreversibility (Surface Annihilation):** You are observing the output of the extraction operator $\Sigma_\pi$, which has physically annihilated the surface form $F(x)$ in volatile memory (`CLFLUSHOPT` zeroization SLA $\le 5.0\text{ ms}$). Mathematical reconstruction of the original text from this representation is prohibited by design:
  $$\forall x \in X, \quad \text{Proj}_F(\Sigma_\pi(x)) = \emptyset$$
* **Semantic Preservation:** Despite the absence of linguistic surface structure, the invariant core $K(x)$—comprising entities, directed causal relations, quantitative coefficients ($e = n/\rho$), and policy-authorized attributes $B_\pi(x)$—is preserved in its entirety.
* **Three-Tier Security Hierarchy:**
  * **Tier 1 (Demonstration / Toy):** Reversible mapping using public constants ($\pi, \phi$). Strictly for pedagogical validation; prohibited for real data.
  * **Tier 2 (Operational Hybrid Compression):** High-density compression ($R_{comp} \ge 26,440\times$) using scope-bound random parameters ($\rho$) stored in isolated private codebooks ($C_{priv}$) with mandatory decoder separation.
  * **Tier 3 (Model-as-Key Bound):** Hardware-isolated TEE execution (Intel SGX2 / AMD SEV-SNP / AWS Nitro) bound to the authorized ensemble $(M, C, \pi, \kappa)$ and cryptographic enclave measurement `MRENCLAVE`.

---

## 2. Control Plane Elements (Top Telemetry Bar)

| Element | Description | Ontological Status |
| :--- | :--- | :--- |
| **`GLYPH_AI_TIER_3_BOUND`** | Indicator of the current security posture. Confirms that data processing occurs within a hardware-isolated Trusted Execution Environment (TEE). | **Axiom 7.1:** Hardware Trust Root |
| **`Axiom E.1: Proj_F(Σ_π(x)) = ∅`** | Formal proof that the projection of transformed data back onto the surface form equals nullity. | **Guarantee of Non-Reversibility** |
| **`Active Codebook: C_dom_v2.1.0`** | Version identifier for the active domain codebook. Defines the dictionary of permissible semantic atoms for the current session. | **Semantic Control Plane** |
| **`Scope ρ: scope_gpu_telemetry_01`** | Identifier for the scope-bound random parameters ($\rho$). Ensures encoding uniqueness specific to this telemetry domain. | **Protection Against Frequency Analysis** |
| **`Waning Crescent (Resting) 25%`** | Visualization of system entropy/uncertainty. A low percentage indicates high determinism in the current ontological state. | **Metric of Ontological Density** |

---

## 3. Repository Publisher Module & File Tree

Designated for institutional pilots and researchers intending to deploy or audit a sovereign GlyphAI node while enforcing the Codebook Orthogonality Axiom ($C_{research} \cap C_{priv} = \emptyset$):

### Option A: Instant 1-Line Terminal Upload
* **Purpose:** Automated deployment of the full Tier 2/Tier 3 stack via CLI.
* **Mechanism:** The `bootstrap.sh` script downloads pre-built Git bundles, initializes the `moon-stone-vault` and `verter-memory` repositories, and configures codebook branching (`main`, `research-codebook`, `private-mappings`).
* **Prerequisites:** Local installation of Git and Bash. Active GitHub CLI session (`gh auth login`).

### Option B: Direct Git Bundle & Archive Downloads
* **Purpose:** Manual deployment or offline architectural audit.
* **Artifacts:**
  * `moon-stone-vault.bundle`: Complete private repository including commit history, Python core, TEE configurations, and `Dockerfile.enclave`.
  * `verter-memory.bundle`: Public repository containing research codebooks (`research-codebook`) and isolated branch definitions.
  * `.tar.gz` archives: Clean source code without Git history for integration into external CI/CD pipelines.

### "Push Both Repos to GitHub" Button
* **Function:** One-click initialization of the dual-repository architecture within your organization, eliminating manual configuration drift.
* **Security:** Utilizes a standard OAuth token with minimal necessary permissions (`repo`, `workflow`) solely for repository creation. The token is never persisted on the server.

### File Tree & Annotated Code Viewer (Bottom Panel)
* **REPOSITORY FILE TREE:** Interactive map of the codebase allowing navigation through `src/ingestion`, `src/encoder`, `src/tee`, and `src/federation`.
* **Code Viewer:** Displays executable source code (e.g., `annihilator.py`) with formal annotations referencing specific monograph chapters and appendices (e.g., `# Appendix E: Surface Annihilation Operator`).

---

## 4. Architecture Navigation (All 12 Interactive Labs)

1. **`Operator Σ_π Ingest`:** Live demonstration of how raw text $x$ is transformed into glyph $g$ via surface annihilation with RAM zeroization.
2. **`Tier 3 Model Barrier`:** Visualization of the Model-as-Key interpretation barrier and `MRENCLAVE` hardware attestation.
3. **`Federated Consortium (Ch. 15.4)`:** Codebook synchronization mechanism between consortium members with disjoint $C_{research}$ and $C_{priv}$ branches.
4. **`Literature Module L_π (Ch. 8)`:** Interface for controlled re-expression $\hat{x}_3 = E_L(g; C, D, \pi)$, generating readable reports from glyphs without reconstructing $F(x)$.
5. **`Codebook Governance (Ch. 6.5)`:** Merkle root $H_{root}(\Delta C)$ epoch ledger, atomic codebook diff auditing, and 2-of-3 TEE enclave multi-sig quorum.
6. **`Institutional Pilot (Phase 4)`:** Readiness red-teaming (Vector $e$), partner onboarding SOP 3.1–3.4, and 5,000-server failure storm benchmark.
7. **`Invariant Topology`:** Interactive graph exploration of invariant semantic atoms $K(x)$ and policy-authorized dimensions $B_\pi(x)$.
8. **`Adversarial Resistance (Axiom E.1)`:** 50,000-trial empirical reconstruction engine proving $0.00\%$ surface recovery, $S_{sem} = 0.998$, and $R_{comp} \ge 26,440\times$.
9. **`Appendix E Formalisation`:** Mathematical monograph proofs and interactive $P_\pi$ policy filter simulation.
10. **`OpenAPI Spec & cURL (v1.0.0)`:** Production REST API contract (`POST /api/v1/ingest`) with copyable cURL, Python, TypeScript, and Go generators.
11. **`Repository Tree (Tier 2/3)`:** Dual-repo publisher, 1-line CLI bootstrapper, portable `.bundle`/`.tar.gz` downloads, and annotated source viewer.
12. **`Gemini Synthesizer (AI Core)`:** Server-side Semantic Oracle querying invariant graphs $G_x = (V_x, E_x)$ with zero raw surface exposure.

---

## 6. Regulatory Compliance Matrix

| Standard / Mandate | Statutory Requirement | Moon-Stone Vault Ontological Guarantee |
| :--- | :--- | :--- |
| **GDPR Art. 17 & 25** | Right to Erasure & Privacy by Design | Surface PII $F(x)$ is annihilated at ingest prior to disk persistence ($\text{Proj}_F = \emptyset$). |
| **HIPAA § 164.514(b)** | Expert Determination De-identification | Pre-image equivalence class $|[x]_{\sim_\pi}| \ge 2^{256}$; direct inversion risk $\approx 0$. |
| **NIST SP 800-88 R1** | Media Sanitization & Memory Purge | `CLFLUSHOPT` volatile buffer overwrite verified within SLA $\le 5.0\text{ ms}$. |
| **SOC 2 Type II** | Processing Integrity & Confidentiality | Continuous CI/CD SHA-256 `MRENCLAVE` registry and 2-of-3 multi-sig governance. |

---

## 7. Frequently Asked Questions (FAQ)

**Q1: Can I reconstruct the original text from the data displayed here (or if the database is stolen)?**  
**A:** No. According to Axiom E.1, the projection onto the surface form is null ($\text{Proj}_F(\Sigma_\pi(x)) = \emptyset$). Data is represented as semantic atoms stripped of their linguistic shell. Controlled re-expression via the Literature Module $L_\pi$ requires private decoder keys and authorized model context, generating a new policy-compliant report $\hat{x}_3$ rather than recovering $F(x)$.

**Q2: Why are two separate repositories required?**  
**A:** This is a mandatory requirement of the Layer Separation Architecture (Chapter 5.3 & Axiom 4.2). The private repository (`moon-stone-vault`) contains transformation logic, TEE enclave definitions, and secrets ($C_{priv}$). The public repositories (`verter-memory:research-codebook` and `moon-stone-vault-ui`) contain only public semantic invariants and compiled static UI assets (`dist/`). Merging them violates the GlyphAI threat model.

**Q3: What is "Scope ρ" (`scope_gpu_telemetry_01`)?**  
**A:** It is a set of random scope-bound parameters ($\rho$) used for homophonic and quantitative encoding ($e = n/\rho$). These parameters are unique to each session/domain and are stored in hardware-sealed private codebooks ($C_{priv}$) separately from the glyphs. Without $\rho$, decoding is mathematically impossible even if the public codebook $C_{dom}$ is known.

**Q4: Is it safe to use the "Push to GitHub" button?**  
**A:** Yes. The process uses the standard GitHub OAuth flow with minimal scopes (`repo`, `workflow`). The server does not retain access to your token after the operation completes, and created repositories inherit your organization's privacy settings.

---

## 8. Monetization & Institutional Engagement (Chapters 11, 12 & Appendix C)

Formal quantification of the Tier 3 commercial model based on Architectural Irreversibility and Sovereign Deployment.

The Moon-Stone Semantic Vault does not monetize through the sale of intellectual property or private decoding keys. Consistent with **Axiom 4.2 (Isolation of Private Contour)** and **Chapter 15.4 (Pathways Toward Standardisation)**, all commercial engagement is structured around access to functionality, compliance guarantees, and sovereign infrastructure. The framework’s value proposition is derived from the fact that raw text $F(x)$ is architecturally annihilated; thus, licensing must be bound to operational access rather than asset transfer.

### Tiered Commercial Architecture

| Tier | Designation | Ontological Basis | Target Segment | Pricing Model |
| :---: | :--- | :--- | :--- | :--- |
| **I** | **Developer Sandbox** | Tier 1 Reversible Demonstration Layer (§5.2). Public codebooks ($C_{pub}$) only. No TEE isolation. | Researchers, Students, Open Source Auditors | Free / Freemium (Rate-limited API) |
| **II** | **Enterprise SaaS** | Tier 2 Hybrid Secure Compression (§5.3). Decoder separation + isolated $C_{priv}$. Multi-tenant TEE enclaves. | Healthcare, Legal, Insurance, EdTech | Subscription (€5k–€50k/mo based on glyph volume & SLA) |
| **III** | **Sovereign Node** | Tier 3 Model-as-Key Bound (§5.4). Full hardware attestation (MRENCLAVE). On-premise/Private Cloud deployment. Codebook governance ownership. | Banks, Government, Defense, Critical Infrastructure | License + Annual Maintenance (€150k–€500k/yr) |

### Compliance as a Product (CaP)
Monetization is directly linked to regulatory risk reduction metrics defined in Chapter 9:
* **GDPR Art. 25 Certification:** Audit-ready proof of Data Protection by Design via architectural non-reversibility. Reduces breach notification liability under Art. 33.
* **HIPAA Safe Harbor Module:** Specialized encoding for PHI ensuring de-identification per §164.514(b) while preserving clinical semantic utility for research (§11.2).
* **NIST AI RMF Alignment:** Mapping of semantic safety gates (§10.6) to NIST SP 800-88 R1 cryptographic sanitization standards.

### Federated Consortium Membership
Based on Chapter 6.5 (Governance of Codebook Ownership), institutions may join the GlyphAI Consortium:
* **Voting Rights:** Influence evolution of domain-specific codebooks ($C_{dom}$).
* **Shared Semantics:** Access to cross-institutional glyph datasets for adversarial validation without exposing raw narratives (§11.5).
* **Standardisation:** Co-authorship of interoperability specs for semantic privacy engineering.

### Dual-Repo Deployment Safety Guarantee
All commercial deployments enforce the Dual-Repo Strategy (visible in Repository Publisher module):
* **Private Core (`moon-stone-vault`):** Contains Python core, TEE configs, $C_{priv}$, and $\rho$ parameters. Never exposed.
* **Public Mirror (`moon-stone-vault-ui`):** Contains only compiled static assets (`dist/`). Hosted freely via GitHub Pages.
* **Commercial Implication:** Clients license the operational capability to ingest and reason over glyphs, not the source code itself. This prevents forking and ensures continuous alignment with security updates.

### Note on Quantum Resilience (§7.4)
Tier 3 pricing includes intrinsic resistance to "harvest now, decrypt later" attacks. Since no reversible ciphertext exists in the primary data object, future quantum advances cannot retroactively expose archived semantic records. This long-term assurance is a distinct component of the Sovereign Node valuation.

**Open Associated Module:** `[Institutional Pilot Console]` | `[Compliance Matrix]` | `[Consortium Registry]`
