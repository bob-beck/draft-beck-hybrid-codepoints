---
title: "TLS 1.3 Code Points for Composite ML-DSA Signature Algorithms"
abbrev: "Composite ML-DSA Code Points for TLS"
category: std

docname: draft-beck-tls-hybrid-codepoints-latest
submissiontype: IETF
number:
date:
consensus: true
v: 3
area: "Security"
workgroup: "Transport Layer Security"
keyword:
 - composite
 - ML-DSA
 - post-quantum
 - code point allocation
venue:
  group: "Transport Layer Security"
  type: "Working Group"
  mail: "tls@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/tls/"
  github: "bob-beck/draft-beck-hybrid-codepoints"
  latest: "https://bob-beck.github.io/draft-beck-hybrid-codepoints/draft-beck-tls-hybrid-codepoints.html"

author:
 -
    fullname: "Bob Beck"
    organization: "OpenSSL"
    email: "beck@obtuse.com"

normative:
  RFC9846:
  I-D.ietf-lamps-pq-composite-sigs:
  RFC9847:

informative:
  RFC5246:
  RFC8017:
  RFC8734:
  RFC9147:
  FIPS204:
    title: "Module-Lattice-Based Digital Signature Standard"
    author:
      - org: "National Institute of Standards and Technology (NIST)"
    date: 2024-08
    seriesinfo:
      FIPS: 204
    target: https://doi.org/10.6028/NIST.FIPS.204

...


--- abstract

This document allocates TLS SignatureScheme code points for the Composite
ML-DSA signature algorithms defined for X.509 in
{{I-D.ietf-lamps-pq-composite-sigs}}, and specifies the parameters an
implementation needs in order to use them interoperably in TLS 1.3.


--- middle

# Introduction

{{I-D.ietf-lamps-pq-composite-sigs}} defines eighteen Composite ML-DSA
signature algorithms, each pairing ML-DSA {{FIPS204}} with a traditional
signature algorithm, and allocates an object identifier for each so that they
can be used in X.509 certificates.

TLS 1.3 {{RFC9846}} negotiates signature algorithms using values from the TLS
SignatureScheme registry. No such values exist for Composite ML-DSA, so these
algorithms cannot be negotiated in TLS even where certificates using them are
available.

This document allocates one code point for each Composite ML-DSA algorithm
defined in {{I-D.ietf-lamps-pq-composite-sigs}}, and specifies the parameters
that the code point alone does not determine.


# Conventions and Definitions

{::boilerplate bcp14-tagged}

"Composite ML-DSA" refers to the signature scheme of
{{I-D.ietf-lamps-pq-composite-sigs}}.


# Composite ML-DSA in TLS 1.3

## SignatureScheme Values

This document adds the following values to the TLS SignatureScheme namespace
defined in Section 4.3.3 of {{RFC9846}}, for use in the
"signature_algorithms" and "signature_algorithms_cert" extensions.

~~~
enum {
    mldsa44_rsa2048_pss_sha256                (TBD1),
    mldsa44_rsa2048_pkcs15_sha256             (TBD2),
    mldsa44_ed25519_sha512                    (TBD3),
    mldsa44_ecdsa_secp256r1_sha256            (TBD4),
    mldsa65_rsa3072_pss_sha512                (TBD5),
    mldsa65_rsa3072_pkcs15_sha512             (TBD6),
    mldsa65_rsa4096_pss_sha512                (TBD7),
    mldsa65_rsa4096_pkcs15_sha512             (TBD8),
    mldsa65_ecdsa_secp256r1_sha512            (TBD9),
    mldsa65_ecdsa_secp384r1_sha512            (TBD10),
    mldsa65_ecdsa_brainpoolP256r1tls13_sha512 (TBD11),
    mldsa65_ed25519_sha512                    (TBD12),
    mldsa87_ecdsa_secp384r1_sha512            (TBD13),
    mldsa87_ecdsa_brainpoolP384r1tls13_sha512 (TBD14),
    mldsa87_ed448_shake256                    (TBD15),
    mldsa87_rsa3072_pss_sha512                (TBD16),
    mldsa87_rsa4096_pss_sha512                (TBD17),
    mldsa87_ecdsa_secp521r1_sha512            (TBD18)
} SignatureScheme;
~~~

Each value corresponds to exactly one algorithm of
{{I-D.ietf-lamps-pq-composite-sigs}}, identified in {{mapping}} by the object
identifier that appears in the SubjectPublicKeyInfo of a certificate carrying a
key for that algorithm. The names are transliterations of the corresponding
Composite ML-DSA algorithm names, using the curve names of the TLS NamedGroup
registry where these differ from those used in
{{I-D.ietf-lamps-pq-composite-sigs}}; the brainpool curve names follow
{{RFC8734}}. Names carry no TLS-level processing semantics.

| Description | Algorithm Identifier |
| --- | --- |
| mldsa44_rsa2048_pss_sha256 | 1.3.6.1.5.5.7.6.37 |
| mldsa44_rsa2048_pkcs15_sha256 | 1.3.6.1.5.5.7.6.38 |
| mldsa44_ed25519_sha512 | 1.3.6.1.5.5.7.6.39 |
| mldsa44_ecdsa_secp256r1_sha256 | 1.3.6.1.5.5.7.6.40 |
| mldsa65_rsa3072_pss_sha512 | 1.3.6.1.5.5.7.6.41 |
| mldsa65_rsa3072_pkcs15_sha512 | 1.3.6.1.5.5.7.6.42 |
| mldsa65_rsa4096_pss_sha512 | 1.3.6.1.5.5.7.6.43 |
| mldsa65_rsa4096_pkcs15_sha512 | 1.3.6.1.5.5.7.6.44 |
| mldsa65_ecdsa_secp256r1_sha512 | 1.3.6.1.5.5.7.6.45 |
| mldsa65_ecdsa_secp384r1_sha512 | 1.3.6.1.5.5.7.6.46 |
| mldsa65_ecdsa_brainpoolP256r1tls13_sha512 | 1.3.6.1.5.5.7.6.47 |
| mldsa65_ed25519_sha512 | 1.3.6.1.5.5.7.6.48 |
| mldsa87_ecdsa_secp384r1_sha512 | 1.3.6.1.5.5.7.6.49 |
| mldsa87_ecdsa_brainpoolP384r1tls13_sha512 | 1.3.6.1.5.5.7.6.50 |
| mldsa87_ed448_shake256 | 1.3.6.1.5.5.7.6.51 |
| mldsa87_rsa3072_pss_sha512 | 1.3.6.1.5.5.7.6.52 |
| mldsa87_rsa4096_pss_sha512 | 1.3.6.1.5.5.7.6.53 |
| mldsa87_ecdsa_secp521r1_sha512 | 1.3.6.1.5.5.7.6.54 |
{: #mapping title="Mapping to Composite ML-DSA Algorithms"}


## Signing and Verification

TLS treats Composite ML-DSA as a single opaque signature algorithm, as it does
Ed25519 and Ed448. The composite construction, its component algorithms, and
its pre-hash are fixed by the negotiated scheme and are specified in
{{I-D.ietf-lamps-pq-composite-sigs}}. The TLS layer neither inspects nor
parameterises them.

When a scheme defined in this document is negotiated, the CertificateVerify
signature is computed over the signing input of Section 4.5.2 of {{RFC9846}}
using Composite-ML-DSA.Sign, and verified using Composite-ML-DSA.Verify
(Sections 3.2 and 3.3 of {{I-D.ietf-lamps-pq-composite-sigs}}).

The context (ctx) parameter MUST be the empty string. Note that the context
parameter of {{I-D.ietf-lamps-pq-composite-sigs}} is different from the context
string of Section 4.5.2 of {{RFC9846}}.


## Restrictions

RSASSA-PKCS1-v1_5 {{RFC8017}} is not defined for use in signed TLS handshake
messages (Section 4.3.3 of {{RFC9846}}). Accordingly,
mldsa44_rsa2048_pkcs15_sha256, mldsa65_rsa3072_pkcs15_sha512, and
mldsa65_rsa4096_pkcs15_sha512 MUST NOT appear in the "signature_algorithms"
extension; they are defined only for the "signature_algorithms_cert" extension
and for signatures in certificates (Section 4.5.1.2 of {{RFC9846}}). An
endpoint that receives a CertificateVerify message using one of these three
schemes MUST abort with an illegal_parameter alert.

The schemes defined in this document MUST NOT be used with TLS 1.2 {{RFC5246}}
or any earlier version. An endpoint that receives a ServerKeyExchange or
CertificateVerify message using one of these schemes in a TLS 1.2 connection
MUST abort with an illegal_parameter alert.

These schemes are equally applicable to DTLS 1.3 {{RFC9147}}.


# Security Considerations

The security considerations of {{I-D.ietf-lamps-pq-composite-sigs}} apply. TLS
authentication requires existential unforgeability under chosen-message attack;
Section 9.2 of {{I-D.ietf-lamps-pq-composite-sigs}} discusses the
unforgeability properties of Composite ML-DSA.


# IANA Considerations

IANA is requested to add the following entries to the TLS SignatureScheme
registry, according to the procedures of Section 16 of {{RFC9847}}. The
Reference for every entry is this document.

| Value | Description | Recommended |
| --- | --- | --- |
| TBD1 | mldsa44_rsa2048_pss_sha256 | N |
| TBD2 | mldsa44_rsa2048_pkcs15_sha256 | N |
| TBD3 | mldsa44_ed25519_sha512 | N |
| TBD4 | mldsa44_ecdsa_secp256r1_sha256 | N |
| TBD5 | mldsa65_rsa3072_pss_sha512 | N |
| TBD6 | mldsa65_rsa3072_pkcs15_sha512 | N |
| TBD7 | mldsa65_rsa4096_pss_sha512 | N |
| TBD8 | mldsa65_rsa4096_pkcs15_sha512 | N |
| TBD9 | mldsa65_ecdsa_secp256r1_sha512 | N |
| TBD10 | mldsa65_ecdsa_secp384r1_sha512 | N |
| TBD11 | mldsa65_ecdsa_brainpoolP256r1tls13_sha512 | N |
| TBD12 | mldsa65_ed25519_sha512 | N |
| TBD13 | mldsa87_ecdsa_secp384r1_sha512 | N |
| TBD14 | mldsa87_ecdsa_brainpoolP384r1tls13_sha512 | N |
| TBD15 | mldsa87_ed448_shake256 | N |
| TBD16 | mldsa87_rsa3072_pss_sha512 | N |
| TBD17 | mldsa87_rsa4096_pss_sha512 | N |
| TBD18 | mldsa87_ecdsa_secp521r1_sha512 | N |
{: #iana-sigschemes title="TLS SignatureScheme Registry Additions"}

IANA is requested to add a comment to the entries for
mldsa44_rsa2048_pkcs15_sha256, mldsa65_rsa3072_pkcs15_sha512, and
mldsa65_rsa4096_pkcs15_sha512 noting that they are defined for use only with
the "signature_algorithms_cert" extension.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
