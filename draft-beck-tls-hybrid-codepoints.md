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
ML-DSA signature algorithms that {{I-D.ietf-lamps-pq-composite-sigs}}
short-lists for implementation, and specifies the parameters an
implementation needs to use them interoperably in TLS 1.3.


--- middle

# Introduction

{{I-D.ietf-lamps-pq-composite-sigs}} defines Composite ML-DSA signature
algorithms, each pairing ML-DSA {{FIPS204}} with a traditional signature
algorithm, and allocates an object identifier for each so that they can be used
in X.509 certificates. Section 10.4 of that document short-lists six of them
for implementation.

TLS 1.3 {{RFC9846}} negotiates signature algorithms using values from the TLS
SignatureScheme registry, which has none for Composite ML-DSA. This document
allocates one code point for each of those six algorithms, and specifies the
parameters that the code point alone does not determine.


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
    mldsa44_ed25519_sha512          (TBD1),
    mldsa44_ecdsa_secp256r1_sha256  (TBD2),
    mldsa65_rsa3072_pss_sha512      (TBD3),
    mldsa65_ecdsa_secp256r1_sha512  (TBD4),
    mldsa65_ed25519_sha512          (TBD5),
    mldsa87_ecdsa_secp384r1_sha512  (TBD6)
} SignatureScheme;
~~~

Each value corresponds to exactly one algorithm of
{{I-D.ietf-lamps-pq-composite-sigs}}. {{mapping}} identifies each by the object
identifier that appears in the SubjectPublicKeyInfo of a certificate carrying a
key for that algorithm. The names transliterate the Composite ML-DSA algorithm
names, using the curve names of the TLS NamedGroup registry where these differ.
Names carry no TLS-level processing semantics.

| Description | Algorithm Identifier |
| --- | --- |
| mldsa44_ed25519_sha512 | 1.3.6.1.5.5.7.6.39 |
| mldsa44_ecdsa_secp256r1_sha256 | 1.3.6.1.5.5.7.6.40 |
| mldsa65_rsa3072_pss_sha512 | 1.3.6.1.5.5.7.6.41 |
| mldsa65_ecdsa_secp256r1_sha512 | 1.3.6.1.5.5.7.6.45 |
| mldsa65_ed25519_sha512 | 1.3.6.1.5.5.7.6.48 |
| mldsa87_ecdsa_secp384r1_sha512 | 1.3.6.1.5.5.7.6.49 |
{: #mapping title="Mapping to Composite ML-DSA Algorithms"}


## Signing and Verification

TLS treats Composite ML-DSA as a single opaque signature algorithm, as it does
Ed25519. The composite construction, its component algorithms, and its pre-hash
are fixed by the negotiated scheme and specified in
{{I-D.ietf-lamps-pq-composite-sigs}}.

When a scheme defined in this document is negotiated, the CertificateVerify
signature is computed over the signing input of Section 4.5.2 of {{RFC9846}}
using Composite-ML-DSA.Sign, and verified using Composite-ML-DSA.Verify
(Sections 3.2 and 3.3 of {{I-D.ietf-lamps-pq-composite-sigs}}).

The context (ctx) parameter MUST be the empty string. The ctx parameter of
{{I-D.ietf-lamps-pq-composite-sigs}} is not the context string of Section 4.5.2
of {{RFC9846}}.


## Restrictions

The schemes defined in this document MUST NOT be used with TLS 1.2 {{RFC5246}}
or any earlier version.

These schemes apply equally to DTLS 1.3 {{RFC9147}}.


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
| TBD1 | mldsa44_ed25519_sha512 | N |
| TBD2 | mldsa44_ecdsa_secp256r1_sha256 | N |
| TBD3 | mldsa65_rsa3072_pss_sha512 | N |
| TBD4 | mldsa65_ecdsa_secp256r1_sha512 | N |
| TBD5 | mldsa65_ed25519_sha512 | N |
| TBD6 | mldsa87_ecdsa_secp384r1_sha512 | N |
{: #iana-sigschemes title="TLS SignatureScheme Registry Additions"}


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
