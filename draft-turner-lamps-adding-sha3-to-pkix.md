---
title: SHA-3 Related Algorithms and Identifiers for PKIX
abbrev: SHA-3 for PKIX
docname: draft-turner-lamps-adding-sha3-to-pkix-latest
date: 2017-03-06
category: std

ipr: trust200902
area: Security
workgroup: Network Working Group
keyword: Internet-Draft

stand_alone: yes
pi: [toc, sortrefs, symrefs]

author:
 -
    ins: S. Turner
    name: Sean Turner
    organization: sn3rd
    email: sean@sn3rd.com

normative:
  RFC2119:
  RFC3279:
  RFC5280:
  RFC5480:
  RFC5912:

  DSS:
	title: "Digital Signature Standard, version 4"
        date: 2013
        author:
          org: National Institute of Standards and Technology, U.S. Department of Commerce
       seriesinfo:
          NIST: FIPS PUB 186-4

  SHA3:
  	title: SHA-3 Standard: Permutation-Based Hash and Extendable-Output Functions
	date: 2015-08
	author:
	  org: National Institute of Standards and Technology, U.S. Department of Commerce
	seriesinfo:
	  NIST: FIPS PUB 202

informative:
  RFC3776:
  RFC4055:
  I-D.curdle-pkix:

--- abstract

This document describes the conventions for using the SHA-3 family of hash functions in the Internet X.509 PKI as one-way hash functions and with the ECDSA signature algorithm; the conventions for the associated ECDSA subject public keys are also described.  Digital signatures are used to sign certificates and CRLs (Certificate Revocation Lists).

--- middle

# Introduction

[RFC3279], [RFC4055], [RFC5480], and [I-D.curdle-pkix] defines the contents of the signatureAlgorithm, signatureValue, signature, and subjectPublicKeyInfo fields within Internet X.509 certificates and CRLs (Certificate Revocation Lists) [RFC5280] for a number of algorithms.  This document does the same for the SHA-3 family of one-way hash functions and their use with the ECDSA and RSA PKCS#1 v1.5 digital signature algorithms.   

Familiarity with [RFC5280] is assumed.

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119].

# Algorithm Support

This section describes cryptographic algorithms which may be used with the Internet X.509 Certificate and CRL profile [RFC5280].  This section describes one-way hash functions and digital signature algorithms which may be used to sign certificates and CRLs, and identifies OIDs (Object Identifiers) for public keys contained in a certificate.

## SHA-3 One-way Hash Functions

The SHA-3 family of one-way hash functions is specified in [SHA3].  In the SHA-3 family, four hash functions are defined: SHA3-224, SHA3-256, SHA3-384, and SHA3-512; two extendable-output functions, called SHAKE128 and SHAKE256, are also defined but are not addressed by this document.  The respective output lengths, in bits, of the SHA-3 hash functions are 224, 256, 384, and 512 and as of this document's publication date correspond to 112, 128, 192, and 256 bits of security [RFC3766].  The OIDs (Object Identifiers) for these four hash functions are as follows:

~~~
  id-sha3-224 OBJECT IDENTIFIER ::= { joint-iso-itu-t(2) country(16)
                                      us(840) organization(1) gov(101)
                                      csor(3) nistAlgorithm(4)
                                      hashAlgs(2) 7 }

  id-sha3-256 OBJECT IDENTIFIER ::= { joint-iso-itu-t(2) country(16)
                                      us(840) organization(1) gov(101)
                                      csor(3) nistAlgorithm(4)
                                      hashAlgs(2) 8 }

  id-sha3-384 OBJECT IDENTIFIER ::= { joint-iso-itu-t(2) country(16)
                                      us(840) organization(1) gov(101)
                                      csor(3) nistAlgorithm(4)
                                      hashAlgs(2) 9 }

  id-sha3-512 OBJECT IDENTIFIER ::= { joint-iso-itu-t(2) country(16)
                                      us(840) organization(1) gov(101)
                                      csor(3) nistAlgorithm(4)
                                      hashAlgs(2) 10 }
~~~ 

When using the id-sha3-224, id-sha3-s256, id-sha3-384, or id-sha3-512 algorithm identifiers, the parameters field MUST be absent; not NULL but absent.

## ECDSA Signature Algorithm with SHA-3

The ECDSA (Elliptic Curve Digital Signature Algorithm) is defined in [SHS].  When ECDSA is used in conjunction with one of the SHA-3 one-way hash functions the OID is, respectively:

~~~
  id-ecdsa-with-sha3-224 ::= { joint-iso-itu-t(2) country(16)
                               us(840) organization(1) gov(101)
                               csor(3) nistAlgorithm(4)
                               sigAlgs(3) 9 }

  id-ecdsa-with-sha3-256 ::= { joint-iso-itu-t(2) country(16)
                               us(840) organization(1) gov(101)
                               csor(3) nistAlgorithm(4)
                               sigAlgs(3) 10 }

  id-ecdsa-with-sha3-384 ::= { joint-iso-itu-t(2) country(16)
                               us(840) organization(1) gov(101) 
                               csor(3) nistAlgorithm(4)
                               sigAlgs(3) 11 }

  id-ecdsa-with-sha3-512 ::= { joint-iso-itu-t(2) country(16)
                               us(840) organization(1) gov(101)
                               csor(3) nistAlgorithm(4)
                               sigAlgs(3) 12 }
~~~

When these algorithm identifiers appear as the algorithm field in an AlgorithmIdentifier, the encoding MUST omit the parameters field.  That is, the AlgorithmIdentifier SHALL be a SEQUENCE of one component: the OBJECT IDENTIFIER id-ecdsa-with-sha3-224, id-ecdsa-with-sha3-256, id-ecdsa-with-sha3-384, or id-ecdsa-with-sha3-512.

The ECParameters in the subjectPublicKeyInfo field of the issuer's certificate SHALL apply to the verification of the signature.

When signing, the ECDSA algorithm generates two values.  These values are commonly referred to as r and s.  To easily transfer these two values as one signature, they MUST be ASN.1 encoded using the ECDSA-Sig-Value defined in [RFC3279] but repeated here for convenience:

~~~
  ECDSA-Sig-Value ::= SEQUENCE {
      r  INTEGER,
      s  INTEGER }
~~~

## ECDSA Public Keys

The conventions for ECDSA public keys is as specified in [RFC5480].  

# Security Considerations

TBD

# IANA Considerations

IANA is kindly requested to register two OIDs in the SMI Security for PKIX Module Identifier registry for the ASN.1 modules found in Appendix A.1 and A.2.  The description is as follows:

* id-mod-pkix1-sha3-2015
* id-mod-pkix1-sha3-1988

where the four digits at the end represent the ASN.1's publication date.

--- back

# 2015 ASN.1 Module

PKIXAlgsForSHA3-2015 { iso(1) identified-organization(3) dod(6)
  internet(1) security(5) mechanisms(5) pkix(7) id-mod(0)
  id-mod-pkix1-sha3-2015(TBD) }

DEFINITIONS EXPLICIT TAGS ::=

BEGIN

-- EXPORTS ALL;

IMPORTS

-- FROM [RFC5912]

PUBLIC-KEY, SIGNATURE-ALGORITHM, DIGEST-ALGORITHM, SMIME-CAPS
SIGNATURE-ALGORITHM, DIGEST-ALGORITHM, SMIME-CAPS
FROM AlgorithmInformation-2009
  { iso(1) identified-organization(3) dod(6) internet(1) security(5)
    mechanisms(5) pkix(7) id-mod(0)
    id-mod-algorithmInformation-02(58) }

-- FROM [RFC5912]

id-ecPublicKey, ECPoint, ECDSA-Sig-Value
PKIXAlgs-2009 { iso(1) identified-organization(3) dod(6)
     internet(1) security(5) mechanisms(5) pkix(7) id-mod(0)
     id-mod-pkix1-algorithms2008-02(56) }

;

-- One-Way Hash Functions

-- SHA3-256

mda-sha3-256 DIGEST-ALGORITHM ::= {
  IDENTIFIER id-sha3-256
  PARAMS ARE absent
  }

id-sha3-256 OBJECT IDENTIFIER ::= { joint-iso-itu-t(2) country(16)
                                    us(840) organization(1) gov(101)
                                    csor(3) nistAlgorithm(4)
                                    hashAlgs(2) 8 }

-- SHA3-384

mda-sha3-384 DIGEST-ALGORITHM ::= {
  IDENTIFIER id-sha3-384
  PARAMS ARE absent
  }

id-sha3-384 OBJECT IDENTIFIER ::= { joint-iso-itu-t(2) country(16)
                                    us(840) organization(1) gov(101)
                                    csor(3) nistAlgorithm(4)
                                    hashAlgs(2) 9 }

-- SHA3-512

mda-sha3-512 DIGEST-ALGORITHM ::= {
  IDENTIFIER id-sha3-512 
  PARAMS ARE absent
  }

id-sha3-512 OBJECT IDENTIFIER ::= { joint-iso-itu-t(2) country(16)
                                    us(840) organization(1) gov(101)
                                    csor(3) nistAlgorithm(4)
                                    hashAlgs(2) 10 }

--
-- Public Key (pk-) Algorithms
--

PublicKeys PUBLIC-KEY ::= {
  ...,
  pk-ec   
  }

-- From [RFC5912] - Here so it compiles.

pk-ec PUBLIC-KEY ::= {
  IDENTIFIER id-ecPublicKey
  KEY ECPoint
  PARAMS TYPE ECParameters ARE required
  -- Private key format not in this module --
  CERT-KEY-USAGE { digitalSignature, nonRepudiation, keyAgreement,
                   keyCertSign, cRLSign }
  }

--
-- Signature Algorithms (sa-)
--

SignatureAlgs SIGNATURE-ALGORITHM ::= {
  ...,
  -- This expands SignatureAlgorithms from [RFC5912]
  sa-ecdsaWithSHA3-256 |
  sa-ecdsaWithSHA3-384 |
  sa-ecdsaWithSHA3-512
  }

--
-- SMIME Capabilities (sa-)
--

SMimeCaps SMIME-CAPS ::= {
  -- The expands SMimeCaps from [RFC5912]
  sa-ecdsaWithSHA3-256.&smimeCaps |
  sa-ecdsaWithSHA3-384.&smimeCaps |
  sa-ecdsaWithSHA3-512.&smimeCaps
  }

-- ECDSA with SHA3-256

sa-ecdsaWithSHA3-256 SIGNATURE-ALGORITHM ::= {
   IDENTIFIER id-ecdsa-with-SHA3-256
   VALUE ECDSA-Sig-Value
   PARAMS TYPE NULL ARE absent
   HASHES { mda-sha3-256 }
   PUBLIC-KEYS { pk-ec }
   SMIME-CAPS { IDENTIFIED BY id-ecdsa-with-SHA3-256 }
   }

id-ecdsa-with-sha3-256 ::= { joint-iso-itu-t(2) country(16)
                             us(840) organization(1) gov(101)
                             csor(3) nistAlgorithm(4)
                             sigAlgs(3) 10 }

-- ECDSA with SHA3-384

sa-ecdsaWithSHA3-384 SIGNATURE-ALGORITHM ::= {
   IDENTIFIER id-ecdsa-with-SHA3-384
   VALUE ECDSA-Sig-Value
   PARAMS TYPE NULL ARE absent
   HASHES { mda-sha3-384 }
   PUBLIC-KEYS { pk-ec }
   SMIME-CAPS { IDENTIFIED BY id-ecdsa-with-SHA3-384 }
   }

id-ecdsa-with-sha3-384 ::= { joint-iso-itu-t(2) country(16)
                             us(840) organization(1) gov(101)
                             csor(3) nistAlgorithm(4)
                             sigAlgs(3) 11 }

-- ECDSA with SHA3-512

sa-ecdsaWithSHA3-512 SIGNATURE-ALGORITHM ::= {
   IDENTIFIER id-ecdsa-with-SHA3-512
   VALUE ECDSA-Sig-Value
   PARAMS TYPE NULL ARE absent
   HASHES { mda-sha3-512 }
   PUBLIC-KEYS { pk-ec }
   SMIME-CAPS { IDENTIFIED BY id-ecdsa-with-SHA3-512 }
   }

id-ecdsa-with-sha3-512 ::= { joint-iso-itu-t(2) country(16)
                             us(840) organization(1) gov(101)
                             csor(3) nistAlgorithm(4)
                             sigAlgs(3) 12 }

END

# 1988 ASN.1 Module

~~~
PKIXAlgsForSHA3-1988 { iso(1) identified-organization(3) dod(6)
  internet(1) security(5) mechanisms(5) pkix(7) id-mod(0)
  id-mod-pkix1-sha3-1988(TBD) }

DEFINITIONS EXPLICIT TAGS ::=

BEGIN

-- EXPORTS ALL;

-- IMPORTS NONE;

--
-- Message Digest Algorithms
--

-- SHA3-256
-- Parameters are absent

id-sha3-256 OBJECT IDENTIFIER ::= { joint-iso-itu-t(2) country(16)
                                    us(840) organization(1) gov(101)
                                    csor(3) nistAlgorithm(4)
                                    hashAlgs(2) 8 }

-- SHA3-384
-- Parameters are absent

id-sha3-384 OBJECT IDENTIFIER ::= { joint-iso-itu-t(2) country(16)
                                    us(840) organization(1) gov(101)
                                    csor(3) nistAlgorithm(4)
                                    hashAlgs(2) 9 }

-- SHA3-512
-- Parameters are absent

id-sha3-512 OBJECT IDENTIFIER ::= { joint-iso-itu-t(2) country(16)
                                    us(840) organization(1) gov(101)
                                    csor(3) nistAlgorithm(4)
                                    hashAlgs(2) 10 }

--
-- ECDSA Keys, Signatures, and Curves
--

-- OID for ECDSA signatures with SHA3-256

id-ecdsa-with-sha3-256 ::= { joint-iso-itu-t(2) country(16)
                             us(840) organization(1) gov(101)
                             csor(3) nistAlgorithm(4)
                             sigAlgs(3) 10 }

-- OID for ECDSA signatures with SHA3-384

id-ecdsa-with-sha3-384 ::= { joint-iso-itu-t(2) country(16)
                             us(840) organization(1) gov(101)
                             csor(3) nistAlgorithm(4)
                             sigAlgs(3) 11 }

-- OID for ECDSA signatures with SHA3-512

id-ecdsa-with-sha3-512 ::= { joint-iso-itu-t(2) country(16)
                             us(840) organization(1) gov(101)
                             csor(3) nistAlgorithm(4)
                             sigAlgs(3) 12 }

-- See [RFC5480] for ECDSA-Sig-Value, which is the format for
-- the value of an ECDSA signature value.

-- See [RFC5480] for ECDSA Keys and Curves. 

END
