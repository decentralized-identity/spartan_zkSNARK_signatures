---
title: "Spartan High-speed zkSNARKs (incubation)"
abbrev: "spartan"
category: info

docname: draft-setty-cfrg-spartan-incubation-latest
ipr: trust200902
area: "IRTF"
workgroup: "Crypto Forum"
keyword: Internet-Draft
venue:
  group: "Crypto Forum"
  type: "Research Group"
  mail: "cfrg@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/search/?email_list=cfrg"
  github: "decentralized-identity/spartan_zkSNARK_signatures"
  latest: "https://decentralized-identity.github.io/spartan_zkSNARK_signatures/draft-setty-cfrg-spartan-incubation.html"

stand_alone: yes
smart_quotes: no
pi: [toc, sortrefs, symrefs]

author:
 -
    name: "Srinath Setty"
    organization: "Microsoft Research"
    email: "srinath@microsoft.com"

normative:

informative:


--- abstract

Spartan is a high-speed zero-knowledge proof system, a cryptographic primitive that enables a prover to prove a mathematical statement to a verifier without revealing anything besides the validity of the statement. 

This work shows how to leverage zkSNARKs to add selective disclosure and unlinkability to verifiable credentials without any trusted setup.

--- middle

# Introduction

References:
* Official Paper: https://eprint.iacr.org/2019/550.pdf
* Open Source Implementation: https://github.com/Microsoft/Spartan
* Draft Repo: https://github.com/decentralized-identity/spartan_zkSNARK_signatures

> This is a work in progress, there are no normative specs here yet.

In an identity system, an issuer (e.g., DMV using Azure AD) issues a credential to a credential holder by digitally signing a set of attribute-value pairs (e.g., name, address, date of birth, photograph, etc.). The credential holder stores such credentials in a wallet app on their mobile devices and can then prove to verifiers (or relying parties) that they hold certain attribute-value pairs issued by a particular issuer. Such credentials are referred to as verifiable credentials, and there is now an emerging W3C standard for issuing such user-controlled, cryptographically verifiable credentials.

Two basic and intuitive privacy guarantees in this context are: (1) Selective Disclosure and (2) Unlinkability. Selective Disclosure means that a credential holder can choose to disclose only a subset of the attribute-value pairs in the credential to a verifier and the verifier learns nothing about the remaining attribute-value pairs in the holder’s credential. For example, if a credential such as a driver’s license includes name, date of birth, full address, and a photograph, the subject can choose to selectively disclose to the verifier their name and ZIP code but nothing else, the name and their state but nothing else, or their name and date of birth but nothing else. Unlinkability means that if the same credential holder presents their credential at two different verifiers, the verifiers—even if they collude—cannot correlate that activity back to the same holder (unless the credential holder reveals uniquely identifying information).

## zkSNARK Signatures

We describe how to add selective disclosure and unlinkability to verifiable credentials issued with standard digital signatures over standardized curves. When a credential holder wishes to provably disclose a subset of attribute-value pairs in their credential to a verifier, the credential holder produces a derived credential containing only the disclosed attribute-value pairs. Such derived credentials are then presented to the verifier who can verify the validity of the credential.

A challenging aspect here is: how does a credential holder produce such derived credentials, with selective disclosure and unlinkability guarantees? Note that the credential holders cannot “call the issuer” for any interaction with verifiers, as this poses additional privacy threats. Furthermore, credential holders should not be able to derive fake credentials. Standard signature schemes such as ECDSA or EdDSA do not support such functionality.

We address both requirements with zkSNARKs, a cryptographic primitive that allows a prover to prove an arbitrary mathematical statement to a verifier. In particular, we plan to leverage Spartan, a high-speed zkSNARK that uses only standardized cryptography and offers state-of-the-art performance. Spartan also does not require any trusted setup (e.g., creating a structured reference string).

An attractive aspect of our approach is that we can retrofit the aforementioned privacy properties to existing verifiable credentials issued with standard signature schemes (e.g., ECDSA over secp256k1 or P-256), with minimal resource overheads and engineering effort. Specifically, an issuer issues credentials as before with a standard signature scheme (e.g., ECDSA) so it is unmodified and incurs no overheads to support privacy features. Deriving a derived credential and verification of a derived credential happens on the credential holder and the verifier side, respectively. Specifically, the credential presentation protocol flow is as follows:
* The issuer digitally signs a set of attribute-value pairs using its private key and sends ( to the holder, where is a digital signature (e.g., ECDSA signature).
* The holder with produces a derived credential where is the disclosed subset of attribute-value pairs and is a proof of knowledge of such that: (i) is valid under the issuer’s public key for , and (ii) is a subset of .
* The verifier with knowledge of the issuer’s public key verifies the derived credential .

# Conventions and Definitions

{::boilerplate bcp14-tagged}


# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
