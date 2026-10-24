9.3. Declaration Criterion and Survey Boundary

This draft makes two class-level claims that require an explicit classification rule and a defined survey boundary to be auditable:

- Abstract: “all except two implementations of early attestation have been archived, withdrawn, or moved to post-handshake attestation.”
- Section 9: “the remaining two implementations of early attestation — Edgeless Systems Contrast and Meta's AI — remain vulnerable.”

Neither claim is accompanied by (a) an explicit declaration criterion specifying what evidence suffices to classify an implementation as vulnerable, or (b) a defined survey boundary specifying the population over which “all except two” is asserted.

Proposed declaration criterion. An implementation of early attestation is classified as vulnerable under this draft if and only if at least one of the following criteria is demonstrated to apply:

- D1 — Binder failure. The binder is not derived from a shared secret, thereby violating G1 as defined in Table 11. This failure must be demonstrated through formal analysis (e.g., [Intra-handshake.fail-repo]) or code inspection establishing the absence of the required secret-dependent derivation.

- D2 — Evidence-data acceptance. The implementation accepts attestation evidence whose "reportData" is nil, empty, or omitted, where the protocol requires this field to bind the evidence to the intended session or challenge. This failure must be demonstrated by a reproducible test case (cf. CVE-2026-92702). The finding applies to an implementation only where the relevant behavior has been independently established; a vulnerability reported in another implementation is not, by itself, proof of the same defect.

- D3 — Identifier trust path. The relying party cannot independently obtain or validate legitimate PIIDs and CHIP_IDs without relying on the vendor as a trusted intermediary. This architectural limitation must be established through an architecture review identifying the identifier-provisioning and validation path, the trust anchors, and the party responsible for asserting identifier legitimacy (cf. Section 9, Contrast). The review must distinguish a documented trust assumption from a demonstrated violation of the security requirements defined by this draft.

- D4 — Policy substitution. The surrounding runtime's generated policies fail to detect one or more unauthorized image substitutions that the policy is intended to prevent. This failure must be demonstrated by a reproducible test case (cf. CVE-2026-100833; GHSA-Edgeless-Systems6). D4 concerns a defect in the surrounding runtime rather than necessarily in the attestation binder itself. It is included because Section 2 establishes that a vulnerability in the overall system, rather than in the attestation mechanism alone, can compromise the security of the system as a whole.

These criteria identify distinct failure modes and must not be treated as interchangeable evidence of the same underlying defect. D1 concerns the cryptographic binding property; D2 concerns evidence-data validation and binding; D3 concerns the architectural trust path for identity; and D4 concerns the enforcement of runtime integrity policy. A finding under any one criterion is sufficient to establish the corresponding security deficiency, provided that the criterion's stated conditions are demonstrated.

An implementation is classified as not demonstrated vulnerable under D1–D4 only when each criterion has been assessed against the relevant implementation and version, and none has been found to apply. A criterion that has not been assessed must be recorded as not assessed, rather than treated as evidence of security. This classification is limited to D1–D4 and does not establish the absence of vulnerabilities outside their scope.

Proposed survey boundary. The claim “all except two” applies to the population of open-source early-attestation implementations surveyed in this draft and enumerated in Table 3 and Section 9.1. The draft should explicitly identify this population as the scope of the authors' survey, rather than imply an exhaustive census of every implementation that may exist. The survey should identify the basis for inclusion and the implementation versions or repository states examined, where available. Consistent with Section 9's existing request for disclosure of undisclosed implementations, the authors should invite corrections, additional implementations, and evidence that may change the classification.

Application to the two retained implementations. Applying the criteria to the findings reported in Section 9 yields the following evidence record:

- Edgeless Systems Contrast: D1 is reported as established (§9, CVE-2026-33697); D3 is reported as established (§9, vendor-mediated identifiers); and D4 is reported as established (CVE-2026-100833, with a fix in version 1.23.1 as reported in §9). D2 is not stated in the cited discussion and therefore remains not established by the evidence presented there. The D4 finding should be scoped to the affected runtime version and should not, by itself, be interpreted as proof of a binder defect in versions containing the fix.

- Meta's AI: D1 is reported as established (§9, CVE-2026-33697). D2, D3, and D4 are not stated in the cited discussion and therefore remain not established by the evidence presented there. Their status must not be inferred from findings concerning Contrast or another implementation without implementation-specific evidence.

Recording which criteria are established, which have been assessed without finding the specified defect, and which remain unassessed converts the Section 9 claim from a generalized assertion into an auditable, implementation-specific classification. It also identifies the relevant remediation target for each demonstrated deficiency without conflating cryptographic, evidence-validation, identity-trust, and runtime-policy failures. Explicitly bounding the survey to the population examined by the authors makes the Abstract's “all except two” claim reproducible and open to correction, without implying a complete census of all existing implementations.  See another 
