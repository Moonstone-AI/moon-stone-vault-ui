# Standard Operating Procedure: Institutional Pilot Onboarding (Moon-Stone Vault)

**Document Version:** 1.0.0  
**Ontological Basis:** GlyphAI Monograph (Ch. 6.5, 7.2, 7.4, 8, 15.4, Appendix E)  
**Target Audience:** Infrastructure Engineers, Security Officers of Consortium Partners  
**Status:** Approved for Phase 4 Institutional Pilot  

---

## 1. Objective
To securely instantiate a new Tier 3 TEE-bound node within the federated consortium, ensuring strict adherence to the Codebook Separation Axiom, Zero-Revelation Protocol, and Architectural Non-Reversibility.

---

## 2. Prerequisites
- **Hardware Enclave:** Dedicated hardware supporting TEE (Intel SGX2 with DCAP 1.14+, AMD SEV-SNP, or AWS Nitro Enclaves).
- **Network Egress:** Network egress allowed *only* to the consortium's attestation registry (`Moon-stone-AI/verter-memory`) and authenticated consortium peer endpoints.
- **Architectural Isolation:** No raw text logs ($F(x)$) shall ever be routed to this node's external interfaces. Perimeter projection annihilates surface representation before any payload crosses the internal boundary.

---

## 3. Onboarding Sequence

### Step 3.1: Environment Initialization
Deploy the verified enclave image. Do not modify the binary, as this will alter the `MRENCLAVE` hash and trigger `403 REMOTE_ATTESTATION_FAILED`.

```bash
docker run -d --device /dev/nitro_enclaves \
  --name moon-stone-node \
  --restart unless-stopped \
  moon-stone-vault:latest
```

Verify that the enclave driver is accessible:
```bash
ls -la /dev/nitro_enclaves # or /dev/sgx_enclave, /dev/sev
```

---

### Step 3.2: Cryptographic Remote Attestation
The node must prove its integrity to the consortium registry before any data exchange.

Execute the local node attestation script within the host environment:
```bash
bash scripts/generate_tee_attestation_cert.sh
```

**Internal Execution Flow:**
1. Generate hardware measurement quote via CPU platform security processor.
2. Query `TRUSTED_MRENCLAVE` from `Moon-stone-AI/verter-memory`:
   $$\text{MRENCLAVE}_{\text{expected}} = \texttt{7f83b1657ff1fc53b92dc18148a1d65dfc2d4b1fa3d677284addd200126d9069}$$
3. **Validation:** The registry verifies the quote against the `TRUSTED_MRENCLAVE` allowlist. Upon success, a short-lived JWT (`MS-TEE.jwt`) is issued.
   - If measurement diverges:
     ```json
     {
       "status": "FATAL_ERROR",
       "error_code": "403 REMOTE_ATTESTATION_FAILED",
       "reason": "Enclave binary fingerprint does not match consensus registry. Execution halted."
     }
     ```

---

### Step 3.3: Sealing of Private Codebook ($C_{\text{priv}}$)
Using the issued JWT (`MS-TEE.jwt`), the node initializes its local, hardware-bound parameters. This step must occur inside the TEE:

1. **Hardware Entropy Sampling:**  
   The CPU TRNG generates a 256-bit homophonic parameter $\rho \in \mathbb{R}^k$.
2. **Hardware Key Sealing (`EGETKEY` / Nitro Sealing Key):**  
   The enclave derives a cryptographic key fused to this physical CPU socket and this specific `MRENCLAVE` measurement:
   $$K_{\text{seal}} \leftarrow \text{DeriveKey}(\text{MRENCLAVE}, \text{SocketID})$$
3. **AES-256-GCM Sealing:**  
   The private codebook $C_{\text{priv}}$ and projection parameter $\rho$ are sealed into non-volatile ciphertext:
   $$\text{Ciphertext} = \text{AES-256-GCM}_{K_{\text{seal}}}(\rho \parallel C_{\text{priv}})$$
   Destination: `/opt/moonstone/codebooks/C_priv/rho.sealed`
4. **Hardware Zeroization:**  
   Plaintext buffers are immediately flushed from volatile cache (`CLFLUSHOPT` / `memset_s`).

> **Ontological Guarantee:** The parameter $\rho$ and private topology mappings are encrypted via AES-256-GCM using a key derived from hardware fuses. The resulting `codebooks/C_priv/rho.sealed` is mathematically inert outside this specific silicon instance.

---

### Step 3.4: Federated Sync Activation
Start the synchronization daemon to pull the public research codebook ($C_{\text{research}}$).

```bash
sudo systemctl enable --now moon-stone-syncd.service
```

**Validation:** The daemon verifies the Merkle root of $C_{\text{research}}$ against the consortium's multi-sig registry (Ch. 6.5). The daemon polls the consortium network and ingests verified updates to $C_{\text{research}}$, while ensuring $C_{\text{research}} \cap C_{\text{priv}} = \emptyset$ at all times.

---

## 4. Operational Constraints
* **Zero Raw Text Retention:** Any process attempting to write raw logs to disk will be terminated by the kernel-level annihilator hook.
* **No Manual Key Extraction:** $C_{\text{priv}}$ cannot be exported, copied, or backed up in plaintext.
* **Audit Trail:** All expansion operations ($E_3$) are logged as topology hashes only, preserving Uncertainty Preservation (Ch. 8).

---

## 5. Emergency Revocation
In the event of suspected compromise, the consortium governance board can revoke the node's `TEE_ID` from the allowlist. Subsequent attestation attempts will be permanently rejected.

---

## 6. Operational Error Codes & Incident Response Matrix

| Error Code | Trigger Condition | Architectural Invariant | Remediation |
| :--- | :--- | :--- | :--- |
| **`403 REMOTE_ATTESTATION_FAILED`** | Local binary SHA-256 $\ne$ `TRUSTED_MRENCLAVE` | Untrusted binary rejection | Rebuild from verified source tagged release. |
| **`MONOTONIC_COUNTER_VIOLATION`** | Hardware anti-replay counter $C_{\text{hw}} > C_{\text{state}}$ | Rollback / state replay attack mitigation | Discard compromised snapshot; trigger re-initialization. |
| **`ONTOLOGICAL_MISMATCH`** | Model topology $M' \ne M_{\text{authorized}}$ evaluated at $E_3$ | Model-as-Key Bound (Chapter 7.4) | Verify client identity and cognitive architecture signature. |
| **`HALLUCINATION_DETECTED`** | Generated atom $v \notin V_x$ in Literature Module | Anti-Hallucination Gate (Chapter 8) | Inspect $L_\pi$ inputs; purge unmapped semantic tokens. |
| **`UNCERTAINTY_GATE_ACTIVE`** | Topological uncertainty $\Omega > \theta_{\text{threshold}}$ | Quinean Ontological Commitment | Invariant working as designed; ambiguous inference blocked. |

---

## 7. Verification & Audit Trail
Following onboarding, inspect the generated node attestation certificate:
```bash
cat attestation_artifacts/pilot_node_attestation.json | jq .
cat attestation_artifacts/MS-TEE.jwt
```

Verify that `attestation_status` is `"CERTIFIED"` and `monotonic_counter_integrity` is `true`.
