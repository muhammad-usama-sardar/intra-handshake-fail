---
title: "Intra-handshake Attestation Considered Harmful (CVE-2026-33697 of CVSS 7.5 and several other CVEs of up to expected CVSS 9.8 upcoming)"
abbrev: "Intra-handshake Attestation Considered Harmful"
category: info

docname: draft-intra-handshake-fail-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
# area: AREA
workgroup: SEAT
keyword:
 - AI agents
 - Intra-handshake attestation
 - CVE-2026-33697
venue:
#  group: WG
#  type: Working Group
#  mail: WG@example.com
#  arch: https://example.com/WG
  github: "muhammad-usama-sardar/intra-handshake-fail"
  latest: "https://muhammad-usama-sardar.github.io/intra-handshake-fail/draft-intra-handshake-fail.html"

author:
 -
    fullname: "Muhammad Usama Sardar"
    organization: TU Dresden, Germany
    email: "muhammad_usama.sardar@tu-dresden.de"
 -
    fullname: "Songbo Bu"
    organization: Shanghai Guan An Information Technology Co., Ltd., China
    email: "bluedognull@gmail.com"
 -
    fullname: "Chengxin Huang"
    organization: Independent
    email: "aurestarnull@gmail.com"
 -
    fullname: "Haowen Song"
    organization: Shanghai Guan An Information Technology Co., Ltd., China
    email: "havan12050544@gmail.com"


normative:
  Intra-handshake.fail:
    title: "Intra-handshake.fail (CVE-2026-33697): High-severity CVE in Attested TLS"
    date: June 2026,
    target: https://www.researchgate.net/publication/408219182_Intra-handshakefail_CVE-2026-33697_High-severity_CVE_in_Attested_TLS
    author:
      - ins: M. U. Sardar
      - ins: V. Dubeyko
      - ins: J-M. Jacquet
  Intra-handshake.fail-repo:
    title: "Intra-handshake.fail (CVE-2026-33697): High-severity CVE in Attested TLS"
    date: July 2026,
    target: https://github.com/muhammad-usama-sardar/intra-handshake.fail
    author:
      - ins: M. U. Sardar
      - ins: V. Dubeyko
      - ins: J-M. Jacquet
  CVE-2026-33697:
     author:
        org: CVE
     title: CoCoS attested TLS is vulnerable to relay attacks via extracted ephemeral TLS keys
     target: https://www.cve.org/CVERecord?id=CVE-2026-33697
     date: 26 March 2026
  EUVD-2026-16488:
     author:
        org: ENISA
     title: CoCoS attested TLS is vulnerable to relay attacks via extracted ephemeral TLS keys
     target: https://euvd.enisa.europa.eu/enisa/EUVD-2026-16488
     date: 26 March 2026

informative:

...

--- abstract

The draft aims to provide technical details of [CVE-2026-33697](https://www.cve.org/CVERecord?id=CVE-2026-33697) and [EUVD-2026-16488](https://euvd.enisa.europa.eu/enisa/EUVD-2026-16488), which is substantial technical evidence of how **intra**-handshake attestation fails in practice, even *without physical access*. Moreover, since continuous attestation is generally required, **intra**-handshake attestation adds **unnecessary complexity**. The results are backed by the research {{Intra-handshake.fail}} and the artifacts {{Intra-handshake.fail-repo}} in state-of-the-art tool, ProVerif, under Apache-2.0 license for reproducibility, and have been acknowledged by the relevant stakeholders.

--- middle

# Introduction

This draft presents the formal specification and analysis of the candidate binding mechanisms for binding in intra-handshake attestation for standardization for attested TLS protocols:

| No. | Binding mechanism | Used in | Artifacts |
| 1. | Client’s TLS nonce | [Meta's AI](https://ai.meta.com/static-resource/private-processing-technical-whitepaper) | [binder1](https://github.com/muhammad-usama-sardar/intra-handshake.fail/tree/main/binder1) |
| 2. | Client’s attestation nonce | - | [binder2](https://github.com/muhammad-usama-sardar/intra-handshake.fail/tree/main/binder2) |
| 3. | Early exporter | - | [binder3](https://github.com/muhammad-usama-sardar/intra-handshake.fail/tree/main/binder3) |
| 4. | Server’s public key | - | [binder4](https://github.com/muhammad-usama-sardar/intra-handshake.fail/tree/main/binder4) |
| 5. | Combination of #2 and #3 | - | [binder5](https://github.com/muhammad-usama-sardar/intra-handshake.fail/tree/main/binder5) |
| 6. | Combination of #2 and #4 | [Edgeless Systems Contrast](https://github.com/CCC-Attestation/meetings/blob/main/materials/MarkusRudy.contrast-atls-ccc-attestation.pdf); [Cocos AI](https://www.sns-itrust6g.com/wp-content/uploads/2025/12/Webinar-Architecting-Trust-CONFIDENTIAL6G.pdf);  [CCC Attestation SIG](https://github.com/CCC-Attestation)'s adopted project [intra-handshake attestation](https://github.com/ccc-attestation/attested-tls-poc) | [binder6](https://github.com/muhammad-usama-sardar/intra-handshake.fail/tree/main/binder6) |
| 7. | Combination of #2, #3, and #4 | [draft-fossati-tls-attestation-06](https://www.ietf.org/archive/id/draft-fossati-tls-attestation-06.html) | [binder7](https://github.com/muhammad-usama-sardar/intra-handshake.fail/tree/main/binder7) |
{: title="Binding mechanisms, implementations and ProVerif artifacts"}

~~~
We provide a formal proof of insecurity of all the above candidate
binding mechanisms of intra-handshake attestation using the
state-of-the-art tool ProVerif and propose a mitigation for the
discovered security vulnerabilities. Our study reveals that it may
not be possible to achieve strong application-traffic (level 3)
binding using intra-handshake attestation alone. This can be exploited
for relay attacks, where an attacker makes a client accept an evidence
from a different machine. So the client cannot be sure that it connects
to its desired server.
~~~


We responsibly disclosed the vulnerability in intra-handshake attestation -- as noted in [security advisory](https://github.com/ultravioletrs/cocos/security/advisories/GHSA-vfgg-mvxx-mgg7) issued -- to the vendors, which resulted in  {{CVE-2026-33697}} of CVSS 7.5.

# Credits

| CVE | Finders |
|---|---|
| CVE-2026-33697 | Muhammad Usama Sardar, Viacheslav Dubeyko, and Jean-Marie Jacquet |
{: title="CVEs and finders"}

# Threat Model
The threat model is explained in Sec. 6.1 of {{Intra-handshake.fail}}.


# Detailed Vulnerability Disclosure Timeline and Public Acknowledgements by Affected Vendors

| Event | Date |
|---|---|
| Our initial responsible disclosure to vendor | 07 Oct, 2025 |
| Acknowledgement by vendor | 14 Dec, 2025 |
| Information to the [IETF](https://mailarchive.ietf.org/arch/msg/rats/6gbqx0XY8WYrH3Mx4vO8n2-uKgY/) | 11 Jan, 2026 |
| [Public announcement](https://web.archive.org/web/20260227160554/https://www.ultraviolet.rs/blog/tee-tls-privacy/) by vendor | 27 Feb, 2026 |
| Cocos AI published [security advisory](https://github.com/ultravioletrs/cocos/security/advisories/GHSA-vfgg-mvxx-mgg7)  [**Severity = HIGH (CVSS 7.8)**] | 23 March, 2026 |
| CVE {{CVE-2026-33697}} published  [**Severity = HIGH (CVSS 7.5)**] | 26 March, 2026 |
| ERISA published EUVD {{EUVD-2026-16488}}  [**Severity = HIGH (CVSS 7.5)**] | 26 March, 2026 |
| [Acknowledgment](https://github.com/Privasys/rustls/releases/tag/privasys-v0.8.1) by Privasys for rustls {{CVE-2026-33697}} [**Severity = HIGH (CVSS 7.5)**] | 9 July, 2026 |
| [Acknowledgment](https://github.com/Privasys/go/releases/tag/privasys-v0.5.1-go1.26.5) by Privasys for go {{CVE-2026-33697}} [**Severity = HIGH (CVSS 7.5)**] | 10 July, 2026 |
| [CCC implementation](https://github.com/ccc-attestation/attested-tls-poc) declared [vulnerable to relay attacks](https://github.com/CCC-Attestation/attested-tls-poc/pull/58) | 17 July, 2026 |
| Vulnerable [CCC implementation repo](https://github.com/ccc-attestation/attested-tls-poc) archived | 22 July, 2026 |
| Vulnerable draft [draft-fossati-tls-attestation](https://datatracker.ietf.org/doc/draft-fossati-tls-attestation/10/) withdrawn by authors |  23 July, 2026 |
| Edgeless Systems published [security advisory](https://github.com/edgelesssys/contrast/security/advisories/GHSA-hjgc-jc5v-fw7h)  [**Severity = HIGH (CVSS 7.4)**] | 29 July, 2026 |
{: title="Detailed vulnerability disclosure timeline and acknowledgements"}

# EU ERISA

European Union's ERISA has independently published {{EUVD-2026-16488}} with CVSS 7.5 to acknowledge this vulnerability.


# Comparison with Other Vulnerabilities in Confidential Computing Literature
{: #sec-cvss-scores }

Severity is based on [NIST metrics](https://nvd.nist.gov/vuln-metrics/cvss).

| Vulnerability | CVE | CVSS | [Severity](https://nvd.nist.gov/vuln-metrics/cvss) |
|---|---|---|---|
| [wiretap.fail](https://wiretap.fail/files/wiretap.pdf) | No CVE ([Intel](https://www.intel.com/content/www/us/en/security-center/announcement/intel-security-announcement-2025-10-28-001.html) and [AMD](https://www.amd.com/en/resources/product-security/bulletin/amd-sb-3040.html) announcements) | - | None |
| [TEE.fail](https://tee.fail/files/paper.pdf) | No CVE | - | None |
| [TDXdown](https://dl.acm.org/doi/10.1145/3658644.3690230) | [Intel](https://www.intel.com/content/www/us/en/security-center/announcement/intel-security-announcement-2024-10-08-001.html) | 2.5 | Low |
| [Staleus](https://xca-attacks.github.io/staleus/staleus_usenix26.pdf) | [CVE-2025-54509](https://www.cve.org/CVERecord?id=CVE-2025-54509) | 4.0 | Medium |
| [BreakFAST](https://xca-attacks.github.io/breakfast/breakfast_oakland26.pdf) | [CVE-2025-61972](https://www.cve.org/CVERecord?id=CVE-2025-6197)| 4.2 | Medium |
| [BadRAM](https://badram.eu/badram.pdf)| [AMD](https://www.amd.com/en/resources/product-security/bulletin/amd-sb-3015.html)| 5.3 | Medium |
| [BreakFAST](https://xca-attacks.github.io/breakfast/breakfast_oakland26.pdf) | [CVE-2025-61971](https://www.cve.org/CVERecord?id=CVE-2025-61971)| 5.9 | Medium |
| [Fabricked](https://xca-attacks.github.io/fabricked/fabricked_usenix26.pdf) | [CVE-2025-54510](https://www.cve.org/CVERecord?id=cve-2025-54510)| 5.9 | Medium |
| [Intra-handshake.fail](https://www.researchgate.net/publication/408219182_Intra-handshakefail_CVE-2026-33697_High-severity_CVE_in_Attested_TLS) | {{CVE-2026-33697}} | 7.5 | High |
{: title="Comparison with other vulnerabilities in confidential computing literature"}

The comparison of the above with CVSS **7.5** for {{Intra-handshake.fail}} indicates that attested TLS is not mature yet compared to the rest of the confidential computing stack, and is currently one of the weakest links in the ecosystem.

# More CVEs

Further formal analysis has led to the following potential CVEs for intra-handshake attestation (currently under disclosure):

- 1 potential CVE of expected CVSS **9.8**
- 2 potential CVEs of expected CVSS **9.1**
- 1 potential CVE of expected CVSS **8.7**
- 2 potential CVEs of expected CVSS **7.5**
- 2 potential CVEs of expected CVSS **7.4**
- 2 potential CVEs of expected CVSS **6.3**

These are preliminary estimates of scores, not final assigned score. They are still under review.

# Vulnerable Implementations

At least the following implementations are vulnerable:

- [Meta's AI](https://ai.meta.com/static-resource/private-processing-technical-whitepaper): {{CVE-2026-33697}} and {{EUVD-2026-16488}} [**Severity = HIGH (CVSS 7.5)**]
- [Cocos AI](https://github.com/ultravioletrs/cocos): [security advisory](https://github.com/ultravioletrs/cocos/security/advisories/GHSA-vfgg-mvxx-mgg7)  [**Severity = HIGH (CVSS 7.8)**], {{CVE-2026-33697}} and {{EUVD-2026-16488}} [**Severity = HIGH (CVSS 7.5)**]
- [Edgeless Systems Contrast](https://github.com/edgelesssys/contrast): [security advisory](https://github.com/edgelesssys/contrast/security/advisories/GHSA-hjgc-jc5v-fw7h)  [**Severity = HIGH (CVSS 7.4)**]
- [CCC Attestation SIG](https://github.com/CCC-Attestation)'s adopted project [intra-handshake attestation](https://github.com/ccc-attestation/attested-tls-poc): declared [vulnerable to relay attacks](https://github.com/CCC-Attestation/attested-tls-poc/pull/58) and **archived**
- Privasys rustls: [Acknowledgment](https://github.com/Privasys/rustls/releases/tag/privasys-v0.8.1) of applicability of {{CVE-2026-33697}} [**Severity = HIGH (CVSS 7.5)**]
- Pirvasys go: [Acknowledgment](https://github.com/Privasys/go/releases/tag/privasys-v0.5.1-go1.26.5) of applicability of {{CVE-2026-33697}} [**Severity = HIGH (CVSS 7.5)**]

If you are aware of any other intra-handshake attestation implementation, please let us know so that we can check and responsibly disclose the vulnerabilities to them.

# Vulnerable Protocol Specifications
At least the following protocol specifications with intra-handshake attestation *path* are vulnerable to {{CVE-2026-33697}} and {{EUVD-2026-16488}}:

- [draft-fossati-tls-attestation](https://datatracker.ietf.org/doc/draft-fossati-tls-attestation/09/): symbolic proof of insecurity; [draft](https://datatracker.ietf.org/doc/draft-fossati-tls-attestation/10/) **withdrawn** after the CVE
- [draft-fossati-seat-early-attestation](https://datatracker.ietf.org/doc/draft-fossati-seat-early-attestation/06/): symbolic and (paper-and-pen-based) computational proof of insecurity (originally done for -04 and applies also to -06)
  - As a SEAT WG participant pointed out, please note that both {{CVE-2026-33697}} and {{EUVD-2026-16488}} contain a link to [security advisory](https://github.com/ultravioletrs/cocos/security/advisories/GHSA-vfgg-mvxx-mgg7) that contains a link to our [SEAT email](https://mailarchive.ietf.org/arch/msg/seat/x3eQxFjQFJLceae6l4_NgXnmsDY/) that contains the G3 property (cf. {{sec-corr-goals}}) that this draft does not satisfy.
  - Some WG participants successfully reproduced the vulnerability by substituting the right value of `rdata` in the shared formal model {{Intra-handshake.fail-repo}} that led to the CVE.
  - An informal reasoning is that binder is not **directly** derived from any **shared secret** in this draft.
- [draft-ritz-seat-facts](https://datatracker.ietf.org/doc/draft-ritz-seat-facts/00/): symbolic proof of insecurity
  - violates G3 property in our analysis
  - unnecessary complexity is itself a security concern

# Binding Levels
1. DH shared secret (`gxy`) used as shared secret between client and server
2. Handshake traffic key (`htsc`) used for encryption of handshake messages
3. Application traffic key (`astc`) used for encryption of application data

Please see Sec. 6.2 of {{Intra-handshake.fail}} for details.

# Security Properties (Correlation Goals)
{: #sec-corr-goals }
We consider TLS Server as RATS Attester, which is typical in confidential computing.

1. Correlation of Evidence to a DH Shared Secret (G1)
2. Correlation of Evidence to Client’s Handshake Traffic Key (G2)
3. Correlation of Evidence to Client’s Application Traffic Key (G3)

Please see Sec. 6.3 of {{Intra-handshake.fail}} for details.

# Main Results

- All analyzed binding mechanisms and the corresponding implementations of intra-handshake attestation are vulnerable to relay attacks.
- Early exporter helps achieve level 1 binding.
- Our proposed mechanism helps achieve level 2 binding.
- It may not be possible to achieve level 3 in intra-handshake attestation alone without additional assumptions.

| Property                   	        | Mechanism #1,2,4,6 | Mechanism #3,5,7 | Proposed mechanism |
| :--                		              | :--    | :--   | :--   |
| G1 : Correlation of Evidence to `gxy` | ❌     | ✅    | ✅    |
| G2 : Correlation of Evidence to `kch` | ❌     | ❌    | ✅    |
| G3 : Correlation of Evidence to `kc`  | ❌     | ❌    | ❌    |
{: title="Main results"}

Please see Sec. 7.1 and Figure 5 of {{Intra-handshake.fail}} for details of attacks.

## Expected Results



| No. | Binding mechanism | Artifacts | Expected results |
|---|---|---|---|
| 1. | Client’s TLS nonce | [binder1](https://github.com/muhammad-usama-sardar/intra-handshake.fail/tree/main/binder1/) | [binder1](https://github.com/muhammad-usama-sardar/intra-handshake.fail/tree/main/binder1/log.txt) |
| 2. | Client’s attestation nonce | [binder2](https://github.com/muhammad-usama-sardar/intra-handshake.fail/tree/main/binder2/) | [binder2](https://github.com/muhammad-usama-sardar/intra-handshake.fail/tree/main/binder2/log.txt) |
| 3. | Early exporter | [binder3](https://github.com/muhammad-usama-sardar/intra-handshake.fail/tree/main/binder3/) | [binder3](https://github.com/muhammad-usama-sardar/intra-handshake.fail/tree/main/binder3/log.txt) |
| 4. | Server’s public key | [binder4](https://github.com/muhammad-usama-sardar/intra-handshake.fail/tree/main/binder4/) | [binder4](https://github.com/muhammad-usama-sardar/intra-handshake.fail/tree/main/binder4/log.txt) |
| 5. | Combination of #2 and #3 | [binder5](https://github.com/muhammad-usama-sardar/intra-handshake.fail/tree/main/binder5/) | [binder5](https://github.com/muhammad-usama-sardar/intra-handshake.fail/tree/main/binder5/log.txt) |
| 6. | Combination of #2 and #4 | [binder6](https://github.com/muhammad-usama-sardar/intra-handshake.fail/tree/main/binder6/) | [binder6](https://github.com/muhammad-usama-sardar/intra-handshake.fail/tree/main/binder6/log.txt) |
| 7. | Combination of #2, #3, and #4 | [binder7](https://github.com/muhammad-usama-sardar/intra-handshake.fail/tree/main/binder7/) | [binder7](https://github.com/muhammad-usama-sardar/intra-handshake.fail/tree/main/binder7/log.txt) |
| 8. | Proposed | [proposal](https://github.com/muhammad-usama-sardar/intra-handshake.fail/tree/main/proposal/) | [proposal](https://github.com/muhammad-usama-sardar/intra-handshake.fail/tree/main/proposal/log.txt) |
{: title="Expected results"}

# Implications of Findings

## Implications of Findings for IETF SEAT WG
- We believe post-handshake attestation alone, such as [draft-fossati-seat-expat](https://datatracker.ietf.org/doc/draft-fossati-seat-expat/), can achieve level 3 binding.
- The research suggests that recent hybrid proposals (combination of intra-handshake attestation and post-handshake attestation) [draft-fossati-seat-early-attestation](https://datatracker.ietf.org/doc/draft-fossati-seat-early-attestation/06/) and [draft-ritz-seat-facts](https://datatracker.ietf.org/doc/draft-ritz-seat-facts/00/) may add **unnecessary complexity** of intra-handshake attestation without adding any security benefit compared to post-handshake attestation alone, such as [draft-fossati-seat-expat](https://datatracker.ietf.org/doc/draft-fossati-seat-expat/). We are not aware of any **security property** that hybrid proposals can achieve that post-handshake attestation alone cannot achieve.
- As demonstrated by our symbolic analysis using ProVerif, the protocol specifications [draft-fossati-seat-early-attestation](https://datatracker.ietf.org/doc/draft-fossati-seat-early-attestation/06/) and [draft-ritz-seat-facts](https://datatracker.ietf.org/doc/draft-ritz-seat-facts/00/) remain vulnerable to CVE-2026-33697. We have also proved that [draft-fossati-seat-early-attestation-04](https://datatracker.ietf.org/doc/draft-fossati-seat-early-attestation/04/) and [draft-fossati-seat-early-attestation-06](https://datatracker.ietf.org/doc/draft-fossati-seat-early-attestation/06/) violate the security theorems in the computational model.

## Implications of Findings for IETF LAKE WG
- Similar problems occur for protocol specification [lake-ra](https://datatracker.ietf.org/doc/draft-ietf-lake-ra/).

## Implications of Findings for IETF TLS WG
- [draft-fossati-tls-attestation-09](https://datatracker.ietf.org/doc/draft-fossati-tls-attestation/09/) is vulnerable to CVE-2026-33697. Thankfully, the authors have withdrawn [draft-fossati-tls-attestation-10](https://datatracker.ietf.org/doc/draft-fossati-tls-attestation/10/).
- Remote attestation *within* the handshake is very dangerous, since to our knowledge, it is one of the highest scored published vulnerabilities in confidential computing literature (see {{sec-cvss-scores}}).

~~~
Given the high-severity vulnerabilities, we recommend that the
developers and maintainers of intra-handshake attestation MUST
urgently move to post-handshake attestation.
~~~

## Implications of Findings for Agent2Agent
From a security perspective, intra-handshake attestation does more damage than protection for AI agents.

# Technical Details

## Tool
We use state-of-the-art symbolic security analysis tool [ProVerif](https://ieeexplore.ieee.org/document/9833653) for the specification of the protocols.

## Modeling

The formal model uses the [fixed version of diversion attacks in intra-handshake attestation](https://github.com/CCC-Attestation/formal-spec-id-crisis/tree/main/TLS-a/fix) from our previous work as the starting point to focus on relay attacks in intra-handshake attestation in this work.
The rationale is that we consider it more useful to show the added value of this contribution to the community by using the [fixed version of diversion attacks in intra-handshake attestation](https://github.com/CCC-Attestation/formal-spec-id-crisis/tree/main/TLS-a/fix) as the baseline, rather than showing the same diversion attacks from [ID-Crisis paper](https://dl.acm.org/doi/10.1145/3779208.3785387), and the discovered CVE ({{CVE-2026-33697}}) -- which the previous analysis could not find -- practically demonstrates the added value.
This modeling choice makes it clear that even with the diversion attacks fixed, high-severity relay attacks would still remain in intra-handshake attestation.

Note: Similar to the [fixed version of diversion attacks in intra-handshake attestation](https://github.com/CCC-Attestation/formal-spec-id-crisis/tree/main/TLS-a/fix) from our previous work, we model non-PSK-based handshake.
From [ID-Crisis paper](https://dl.acm.org/doi/10.1145/3779208.3785387):

> For modeling TLS 1.3, we consider handshakes based on Diffie-Hellman over either finite fields or elliptic curves, represented as (EC)DHE. This is because we are unaware of any publicly available specification or implementation of attested TLS with PSK-based handshakes.

While it would be nice to model PSK-based handshake, the rationale is that the correlation properties studied in this work do not necessarily require it.

Note: The artifacts consider the case of server authentication only, as client authentication is optional in TLS 1.3. No claims are made about other configurations.

## Technical Report
Technical report is available at {{Intra-handshake.fail}}. It is accepted for publication at ESORICS 2026.

### Vulnerabilities
Sec. 7.1 of {{Intra-handshake.fail}} presents the technical details with abstract attack traces of the vulnerabilities.

### Mitigation
Sec. 7.2 of {{Intra-handshake.fail}} presents the technical details of the proposed mitigation.

## Artifacts
Artifacts are available at {{Intra-handshake.fail-repo}} under Apache-2.0 License.

# Media Coverage

Several media enthusiasts and bloggers have covered the vulnerabilities to protect the community from the harm of intra-handshake attestation.

- [The Register](https://www.theregister.com/security/2026/07/04/confidential-computings-trust-mechanism-is-broken-the-fix-may-not-exist/5266056)
- (Japanese) [BlackHatNewsTokyo](https://blackhatnews.tokyo/archives/119915)
- (Several languages) [Hackernoon](https://hackernoon.com/attested-tls-was-supposed-to-be-the-last-trust-boundary-it-isnt-formal-methods-show-how)
- [Apple podcast](https://podcasts.apple.com/eg/podcast/attested-tls-was-supposed-to-be-the-last-trust/id1698517643?i=1000776623286)
- [Information Security News](https://meterpreter.org/attested-tls-vulnerability-cve-2026-33697/)
- [TheNextGenTechInsider](https://thenextgentechinsider.com/pulse/critical-flaw-discovered-in-confidential-computing-attestation-protocols)
- [DailySecurityReview](https://dailysecurityreview.com/resources/cve-2026-33697-attested-tls-relay-flaw-hits-whatsapp-cocos-ai/)
- [SC World](https://www.scworld.com/brief/confidential-computings-remote-attestation-protocol-may-have-fundamental-flaw)
- [01 Quantum](https://blogs.groupware.org.uk/01-Quantum-Inc/the-handshake-that-cant-keep-its-promise-why-confidential-computings-flaw-changes-the-data-sovereignty-conversation/)
- (Russian) [Security Lab](https://www.securitylab.ru/news/574545.php)
- (German) [blogspan](https://www.blogspan.net/confidential-computing-attestierung-relay-luecke/)
- (Chinese) [Sina](https://finance.sina.cn/tech/2026-07-04/detail-inifscxt9953361.d.html)
- [data4biz](https://data4biz.com/articles/una-falla-rompe-la-fiducia-del-confidential-computing)
- (Russian) [ITSec](https://www.itsec.ru/news/issledovateli-nashli-kriticheskuyu-uyazvimost-v-attested-tls)
- (Chinese) [smzdm](https://post.smzdm.com/p/a82ol990/)
- (Chinese) [donews](https://www.donews.com/news/detail/4/6621022.html)
- (Chinese) [ifeng](https://i.ifeng.com/c/8uUfy0PMmqE)
- [dugganusa](https://www.dugganusa.com/post/confidential-computing-s-whole-pitch-is-trust-the-proof-not-the-cloud-two-years-of-formal-verifi)
- [dugganusa repo](https://github.com/pduggusa/dugganusa-ietf/tree/main/cve-2026-33697-attestation)
- [spoitus](https://sploitus.com/exploit?id=92591A05-07BC-5015-BA3D-B1347B35D684)
- [lavx news](https://news.lavx.hu/article/attested-tls-research-exposes-a-weak-link-in-confidential-computing)
- [sohu](https://www.sohu.com/a/1045865934_122004016)
- (Persian) [news.ditty](https://news.ditty.ir/news/attested-tls-relay-flaw-formal-methods/019f6221-26ca-7293-9ee9-5557b3c0b8f8)
- (Russian) [LiMP VPN](https://limpvpn.com/ru/news/attested-tls-whatsapp-privacy-flaw-2026)
- [daily.dev](https://daily.dev/posts/kI6PoNzPx)
- [warden](https://warden.veritai.ch/news/researchers-find-attested-tls-flaws-that-weaken-confidential-computing-trust-model)
- [GCVE.eu](https://db.gcve.eu/sightings/?query=cve-2026-33697)
- [coderlegion](https://coderlegion.com/24087/intra-handshake-attestation-when-more-security-doesnt-mean-better-security)
- [Anjuna Security](https://www.anjuna.io/blog/attested-tls-flaw-explained)
- [freenode](https://freenode.net/digest/67)
- (Chinese) [csdn](https://blog.csdn.net/weixin_42376192/category_13096766.html)
- [osintsights](https://osintsights.com/confidential-computing-flaws-expose-trust-risks)
- (Turkish) [hardwaremania](https://hardwaremania.com/haber/arastirma-attested-tls-confidential-computing-icin-zayif-kaliyor/)
- [akber](https://akber.com/sovereignty-in-the-cloud-is-an-illusion/)
- [ad-hoc news](https://www.ad-hoc-news.de/wissenschaft/cloud-souveraenitaet-red-hat-startet-reifegrad-assessments-gegen/69691475)
- [AIMultiple](https://aimultiple.com/privacy-enhancing-technologies)

If you have written an article on this and would like to be added here, please send us a PR or an email with the subject "media coverage of intra-handshake.fail"

## Security Researchers

Several credible security researchers, such as the following, have publicly attested to it.

- [Michael Pak](https://www.linkedin.com/posts/michaelpak_confidential-computings-core-trust-mechanism-activity-7479415537836376064-q-A4/)
- [Rodrigo Branco](https://www.linkedin.com/posts/rrbranco_one-more-evidence-that-there-is-no-such-a-share-7479582122366615552-X0A5/)
- [Bart Preneel](https://www.linkedin.com/posts/bart-preneel-4451412_on-the-limits-of-confidential-computing-share-7479549718294077440-wfi3/)
- [Thorsten Strufe](https://www.linkedin.com/in/strufe/recent-activity/all/)

## Germany's BSI

Germany's Federal Office for Information Security (Bundesamt für Sicherheit in der Informationstechnik) has attested to it. Carina Hilt, deputy press spokesperson at BSI, told [The Register](https://www.theregister.com/security/2026/07/04/confidential-computings-trust-mechanism-is-broken-the-fix-may-not-exist/5266056):

~~~
CC alone cannot satisfy the requirements for digital sovereignty.
~~~

~~~
dependencies on other services, such as identity and key
management etc., are also not mitigated by CC.
~~~

CC refers to Confidential Computing, and attested TLS is the core trust mechanism of CC.

# Reviews

## Conference Reviews
{{Intra-handshake.fail}} has been peer-reviewed and accepted for publication at ESORICS 2026.

## IETF/IRTF
Several participants of the IETF/IRTF have attested to the results by independently reproducing the results and reviewing the code. Some of the participants have independently reproduced the results by developing their own formal models and a proof-of-concept implementation of the vulnerabilities. Some of the messages are mentioned below:

- [https://mailarchive.ietf.org/arch/msg/seat/B7F1Dj_rjs8I0Kg3yCp3Rap0XeE/](https://mailarchive.ietf.org/arch/msg/seat/B7F1Dj_rjs8I0Kg3yCp3Rap0XeE/)
- [https://mailarchive.ietf.org/arch/msg/seat/aEV9dUFotAQzHndk23qBcwBT3as/](https://mailarchive.ietf.org/arch/msg/seat/aEV9dUFotAQzHndk23qBcwBT3as/)
- [https://mailarchive.ietf.org/arch/msg/seat/3Hv0E1sfXsvyBtl6AgY8j-SHHiw/](https://mailarchive.ietf.org/arch/msg/seat/3Hv0E1sfXsvyBtl6AgY8j-SHHiw/)
- [https://mailarchive.ietf.org/arch/msg/seat/5LJ6i9svomnhpyPHWPejM7fMmXQ/](https://mailarchive.ietf.org/arch/msg/seat/5LJ6i9svomnhpyPHWPejM7fMmXQ/)
- [https://mailarchive.ietf.org/arch/msg/seat/V_YqGUY3fEwaFwwyfA9DshpHet0/](https://mailarchive.ietf.org/arch/msg/seat/V_YqGUY3fEwaFwwyfA9DshpHet0/)
- [https://mailarchive.ietf.org/arch/msg/seat/JF_cwmHHEbrJ_W5V6yEetWWnii4/](https://mailarchive.ietf.org/arch/msg/seat/JF_cwmHHEbrJ_W5V6yEetWWnii4/)
- [https://mailarchive.ietf.org/arch/msg/seat/P_CYTycg0KG7cbKauFA-kVgX2jo/](https://mailarchive.ietf.org/arch/msg/seat/P_CYTycg0KG7cbKauFA-kVgX2jo/)
- [https://mailarchive.ietf.org/arch/msg/seat/ZJjJXpYwZ5nCVmz_W4FK6XiFEY4/](https://mailarchive.ietf.org/arch/msg/seat/ZJjJXpYwZ5nCVmz_W4FK6XiFEY4/)
- [https://mailarchive.ietf.org/arch/msg/seat/4so3LxHOOXHS1wnvhuoeWWgeCHk/](https://mailarchive.ietf.org/arch/msg/seat/4so3LxHOOXHS1wnvhuoeWWgeCHk/)
- [https://mailarchive.ietf.org/arch/msg/seat/Q6Jmc58v0c1lDV3ujIY0AX_ofGA/](https://mailarchive.ietf.org/arch/msg/seat/Q6Jmc58v0c1lDV3ujIY0AX_ofGA/)
- [https://mailarchive.ietf.org/arch/msg/seat/n4Me5QPCvwhxcEJndWePyishcoo/](https://mailarchive.ietf.org/arch/msg/seat/n4Me5QPCvwhxcEJndWePyishcoo/)
- [https://mailarchive.ietf.org/arch/msg/seat/beRzNNvwMifkRfJPfxecGoHpTDs/](https://mailarchive.ietf.org/arch/msg/seat/beRzNNvwMifkRfJPfxecGoHpTDs/)
- [https://mailarchive.ietf.org/arch/msg/cfrg/U5YHd91lYjiqCTt9BZyVDNFeUpM/](https://mailarchive.ietf.org/arch/msg/cfrg/U5YHd91lYjiqCTt9BZyVDNFeUpM/)
- [https://mailarchive.ietf.org/arch/msg/seat/aFCo4BMRDSUynvN9AQJatPjnXag/](https://mailarchive.ietf.org/arch/msg/seat/aFCo4BMRDSUynvN9AQJatPjnXag/)
- [https://mailarchive.ietf.org/arch/msg/seat/wb_Ys9MZd9u9oM2Bk-8tv7fvXGg/](https://mailarchive.ietf.org/arch/msg/seat/wb_Ys9MZd9u9oM2Bk-8tv7fvXGg/)
- [https://mailarchive.ietf.org/arch/msg/seat/ov8f-7cZKK5RZ-Mmjc6IhVSB-Fk/](https://mailarchive.ietf.org/arch/msg/seat/ov8f-7cZKK5RZ-Mmjc6IhVSB-Fk/)
- [https://mailarchive.ietf.org/arch/msg/seat/2uUuaD1DygjNDM4GT_-rYbwTiJk/](https://mailarchive.ietf.org/arch/msg/seat/2uUuaD1DygjNDM4GT_-rYbwTiJk/)
- [https://mailarchive.ietf.org/arch/msg/seat/pB39abN1QrH4_ATM_E78vxPTuxk/](https://mailarchive.ietf.org/arch/msg/seat/pB39abN1QrH4_ATM_E78vxPTuxk/)
- [https://mailarchive.ietf.org/arch/msg/seat/PxKCxMHe-SAiR9uhOOllrK4mUA4/](https://mailarchive.ietf.org/arch/msg/seat/PxKCxMHe-SAiR9uhOOllrK4mUA4/)
- [https://mailarchive.ietf.org/arch/msg/seat/T1xupUBwqYEBSHCTXgSHXZtdqz8/](https://mailarchive.ietf.org/arch/msg/seat/T1xupUBwqYEBSHCTXgSHXZtdqz8/)
- [https://mailarchive.ietf.org/arch/msg/seat/hRw46FwgmVdi9fqZm2fjKbln_IA/](https://mailarchive.ietf.org/arch/msg/seat/hRw46FwgmVdi9fqZm2fjKbln_IA/)
- [https://mailarchive.ietf.org/arch/msg/seat/UG7yE_klmRSxNy2HX6fzuFonDjM/](https://mailarchive.ietf.org/arch/msg/seat/UG7yE_klmRSxNy2HX6fzuFonDjM/)
- [https://mailarchive.ietf.org/arch/msg/seat/2hpeIldeFfE6o9q6L9Vkt00ACKA/](https://mailarchive.ietf.org/arch/msg/seat/2hpeIldeFfE6o9q6L9Vkt00ACKA/)
- [https://mailarchive.ietf.org/arch/msg/seat/gc2ij0vboehS_-v10-SNslxaZC0/](https://mailarchive.ietf.org/arch/msg/seat/gc2ij0vboehS_-v10-SNslxaZC0/)
- [https://mailarchive.ietf.org/arch/msg/seat/oO4mAfq5HJZptDDNrSnd7zDdX18/](https://mailarchive.ietf.org/arch/msg/seat/oO4mAfq5HJZptDDNrSnd7zDdX18/)
- [https://mailarchive.ietf.org/arch/msg/seat/XuJc_yEJPCMIuYcv2OM7XDogRCU/](https://mailarchive.ietf.org/arch/msg/seat/XuJc_yEJPCMIuYcv2OM7XDogRCU/)
- [https://mailarchive.ietf.org/arch/msg/seat/nVHlnbFIEh-cPQeMuDVOqx5YvWQ/](https://mailarchive.ietf.org/arch/msg/seat/nVHlnbFIEh-cPQeMuDVOqx5YvWQ/)
- [https://mailarchive.ietf.org/arch/msg/seat/xVU3C7qUOngcip7B4ZO5MJUT9Xg/](https://mailarchive.ietf.org/arch/msg/seat/xVU3C7qUOngcip7B4ZO5MJUT9Xg/)
- [https://mailarchive.ietf.org/arch/msg/seat/1gCcPw-7NopDRzzBzA3dFIgo3Rs/](https://mailarchive.ietf.org/arch/msg/seat/1gCcPw-7NopDRzzBzA3dFIgo3Rs/)
- [https://mailarchive.ietf.org/arch/msg/seat/t8aobzB374lWiLzrVrORY7kGYyQ/](https://mailarchive.ietf.org/arch/msg/seat/t8aobzB374lWiLzrVrORY7kGYyQ/)
- [https://mailarchive.ietf.org/arch/msg/seat/m3UyB6XLQzxaucejE_o8Pn41uSI/](https://mailarchive.ietf.org/arch/msg/seat/m3UyB6XLQzxaucejE_o8Pn41uSI/)
- [https://mailarchive.ietf.org/arch/msg/seat/gqHqcbbKva_oGE-jEDZu243gf-4/](https://mailarchive.ietf.org/arch/msg/seat/gqHqcbbKva_oGE-jEDZu243gf-4/)
- [https://mailarchive.ietf.org/arch/msg/seat/QD8QB1WVL-toNovGQ2Tk6DmmeEM/](https://mailarchive.ietf.org/arch/msg/seat/QD8QB1WVL-toNovGQ2Tk6DmmeEM/)
- [https://mailarchive.ietf.org/arch/msg/seat/vXN2pifZ5GXcC1xLwSfLCnUcFUE/](https://mailarchive.ietf.org/arch/msg/seat/vXN2pifZ5GXcC1xLwSfLCnUcFUE/)
- [https://mailarchive.ietf.org/arch/msg/seat/MGFXinb85XSaLkqjBEhmBZC7PcI/](https://mailarchive.ietf.org/arch/msg/seat/MGFXinb85XSaLkqjBEhmBZC7PcI/)
- [https://mailarchive.ietf.org/arch/msg/seat/js9VI4PB8yYmhg2ObaZB1a22fL4/](https://mailarchive.ietf.org/arch/msg/seat/js9VI4PB8yYmhg2ObaZB1a22fL4/)
- [https://mailarchive.ietf.org/arch/msg/seat/0RzORzX_VdlY5UQ_MWMmxZlnrjs/](https://mailarchive.ietf.org/arch/msg/seat/0RzORzX_VdlY5UQ_MWMmxZlnrjs/)
- [https://mailarchive.ietf.org/arch/msg/seat/W3MH1BDSUbm1WUPxGQihaIc1zTk/](https://mailarchive.ietf.org/arch/msg/seat/W3MH1BDSUbm1WUPxGQihaIc1zTk/)
- [https://mailarchive.ietf.org/arch/msg/seat/2_aGylmFHoLmqN7BBcYVoH-rNJk/](https://mailarchive.ietf.org/arch/msg/seat/2_aGylmFHoLmqN7BBcYVoH-rNJk/)
- [https://mailarchive.ietf.org/arch/msg/seat/VyifG8zP5aworb_S1NR9FEUEXW0/](https://mailarchive.ietf.org/arch/msg/seat/VyifG8zP5aworb_S1NR9FEUEXW0/)
- [https://mailarchive.ietf.org/arch/msg/seat/CYwvM75z6rTId2A3ZZvZJmHxOig/](https://mailarchive.ietf.org/arch/msg/seat/CYwvM75z6rTId2A3ZZvZJmHxOig/)
- [https://mailarchive.ietf.org/arch/msg/ufmrg/29xFZX5C4oSGkpZAvXT_7YLW2Vc/](https://mailarchive.ietf.org/arch/msg/ufmrg/29xFZX5C4oSGkpZAvXT_7YLW2Vc/)
- [https://mailarchive.ietf.org/arch/msg/seat/u1HxYW9cJfVpi3Cf9q06ehwpYGE/](https://mailarchive.ietf.org/arch/msg/seat/u1HxYW9cJfVpi3Cf9q06ehwpYGE/)
- [https://mailarchive.ietf.org/arch/msg/seat/SG_A0016a-KMnXAkGtMUxokZmjc/](https://mailarchive.ietf.org/arch/msg/seat/SG_A0016a-KMnXAkGtMUxokZmjc/)
- [https://mailarchive.ietf.org/arch/msg/seat/3w7-OW2CAVr0-QBz97eAMxB_nMI/](https://mailarchive.ietf.org/arch/msg/seat/3w7-OW2CAVr0-QBz97eAMxB_nMI/)

### Main Questions

In short, four main questions have been raised by WG participants in support of our work:

- What **security property** hybrid (intra- + post-handshake attestation) provides that post-handshake attestation alone cannot provide?
- Since continuous attestation is required in most use cases, how is **additional complexity** of **intra**-handshake attestation justified? Use cases with one-time attestation can be covered by doing attestation round immediately after Connection Establishment Time: see [reference](https://www.ietf.org/archive/id/draft-usama-seat-intra-vs-post-04.html#section-6-2).
- What is the benefit of doing **signatures** of remote attestation **within** the handshake (as this latency can be exploited)? We add that **verification** of signatures is also time consuming, which can be exploited too. See [reference](https://www.ietf.org/archive/id/draft-usama-seat-intra-vs-post-04.html#section-4.2.4).
- How evidence is bound to the secure channel without involving any **shared secret**?

## Researchers outside of IETF/IRTF

Some researchers have approached us confirming the proof-of-concept of the vulnerabilities in intra-handshake attestation. More information will be added once their pre-prints/papers are public.

# Security Considerations

All of this document is about the **insecurity** of **intra**-handshake attestation.

By no means should the vendors mentioned in this draft be considered less secure than any other vendors implementing intra-handshake attestation solutions. In particular, those who have closed-source implementations are most likely more vulnerable than the open-source ones, since the former cannot easily be reviewed by the security community. Even extensive security reviews -- of closed-source implementations -- by cybersecurity firms often do not perform formal analysis, and thus such reviews may miss corner cases and subtle vulnerabilities.

# Ethical Considerations

We (i.e., the super set of all authors involved in this research) are ethical researchers aiming to protect the community from the potential harm caused by the exploitability of the vulnerabilities in intra-handshake attestation. We have responsibly disclosed the vulnerabilities to the respective developers and maintainers following their respective disclosure processes and provided them our proposed mitigations and requested them to take rapid action.

We have released only the formal analysis for published CVE. To minimize exploit in the wild, we have not publicly released the proof-of-concept exploit code.

We have not retrieved any real data from any real system. We have not released any key to any public forum or to any person.

To the best of our abilities, knowledge, and understanding, we have tried to explain the vulnerabilities to the authors of vulnerable drafts [draft-fossati-tls-attestation](https://datatracker.ietf.org/doc/draft-fossati-tls-attestation/09/), [draft-fossati-seat-early-attestation](https://datatracker.ietf.org/doc/draft-fossati-seat-early-attestation/06/), and [draft-ritz-seat-facts](https://datatracker.ietf.org/doc/draft-ritz-seat-facts/00/) for at least half a year at several forums, including but not limited to CCC Attestation SIG and IETF/IRTF. Please see the (non-exhaustive list of) [recordings](https://github.com/muhammad-usama-sardar/intra-handshake.fail#upcoming-and-recent-talks-and-research-visits) and the [archives](https://github.com/muhammad-usama-sardar/intra-handshake.fail#community-service). We sincerely thank the authors of [draft-fossati-tls-attestation](https://datatracker.ietf.org/doc/draft-fossati-tls-attestation/10/) for withdrawing their draft to protect further exploits.

# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

We would like to thank our co-authors of paper {{Intra-handshake.fail}} for their valuable contributions:

- Viacheslav Dubeyko
- Jean-Marie Jacquet

We gratefully acknowledge the following for insightful discussions and reviews on this work:

- Eric Rescorla
- Juho Forsén
- Markus Rudy
- Mariam Moustafa
- Bruno Blanchet
- Steve Kremer
- Tjaden Hess
- Martin Thomson
- Yuning Jiang
- Pavel Nikonorov
- Casey Wilson
- Danko Miladinovic
- John Preuß Mattsson
- Britta Hale
- Werner Staub
- Haowen Song
- Chengxin Huang
- Steve Luo
- Kubilay Ahmet Küçük
- Iman Schrock
- Sophie Schmieg
- Jakub Maria Plutowski
- Patrick Duggan
- Nathanael Ritz
- Deb Cooley

We also gratefully acknowledge the following who gave feedback on [previous state-of-the-art](https://github.com/CCC-Attestation/formal-spec-id-crisis) that we utilize as the basis:

- Tuomas Aura
- Ionut Mihalcea
- Thomas Fossati
- Hannes Tschofenig
- Yaron Sheffer
- Laurence Lundblade
- Giridhar Mandyam
- Christopher Patton
- Jonathan Hoyland
- Richard Barnes

Several others at the IETF, IRTF, CCC, and GA4GH have contributed by providing feedback.

We sincerely thank Karthikeyan Bhargavan, Bruno Blanchet, and Nadim Kobeissi for the foundational formal model of draft 20 of TLS 1.3 in their [work](https://ieeexplore.ieee.org/document/7958594).

Muhammad Usama Sardar is funded by German Research Foundation ("Deutsche Forschungsgemeinschaft.")
