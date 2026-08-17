# machinery-semantics

Reproducible audits of the open standards machine builders will rely on to meet
EU Machinery Regulation (EU) 2023/1230.

## The deadline

The Regulation **applies from 14 January 2027**, the same day Directive 2006/42/EC is repealed.
Verified against the EUR-Lex primary text, Article 54. Several vendor summaries in circulation
give 20 January and are wrong. Articles 26 to 42 have applied since 14 January 2024.

## The finding

MTConnect publishes every version as both an XSD and a JSON Schema, which makes the two
mutually checkable.

- JSON Schema `AlarmStateEnum` = `INSTANT`, `ACTIVE`, `CLEARED`
- XSD `AlarmStateType` = `ACTIVE`, `CLEARED`
- The string `INSTANT` appears nowhere in either XSD document
- Identical in versions 2.0, 2.1 and 2.2

A JSON-side implementation may legitimately emit `INSTANT` and an XSD-validating consumer will
reject it.

## A retracted finding, published deliberately

Our first pass reported **111 divergences**. That number was wrong.

- **105** were the `UNAVAILABLE` sentinel present in XSD enumerations but not JSON ones. The JSON
  Schema handles unavailability structurally via an `isUnavailable` flag and a separate value
  constraint. This is a deliberate design difference, not a defect.
- **3** were an artefact of our own name normalisation, which collapsed two genuinely distinct XSD
  types, `CoordinateSystemEnumType` and `CoordinateSystemTypeEnumType`, onto one key across a
  Devices/Streams file boundary.

One real defect remains, across three releases.

## A common claim we tested and refuted

Asset Administration Shell submodel templates are often said to require a paid ECLASS or IEC CDD
subscription to resolve field semantics. Across the IDTA published templates we counted
**1,529 references to IDTA's own IRIs against 18 ECLASS and 9 IEC CDD**. The templates are
overwhelmingly self-referential. Compliance does not require a dictionary subscription.

## Gotcha for anyone reproducing this

The MTConnect XSDs use **single-quoted XML attributes**. A regular expression written for double
quotes returns zero matches and looks like a clean result.

## Sources and licences

- MTConnect schemas, `mtconnect/schema`. Repository licence Apache 2.0; the XSD files themselves
  carry an AMT BSD-3-Clause notice. Both are recorded here because they differ.
- IDTA submodel templates, `admin-shell-io/submodel-templates`, CC BY 4.0, 54 published templates.
- OPC UA `OPCFoundation/UA-Nodeset` was **excluded**: it has no LICENSE file and states no terms.
- SEMI E5 and E30 were **excluded**: paid store products, so their text cannot enter an ontology.

## Articles

English, Korean and Traditional Chinese write-ups are in [`articles/`](articles/) and published at
[gov.tesseract.academy](https://gov.tesseract.academy/research).

## Contact

Kampakis and Co Ltd, trading as The Tesseract Academy. fabio@thetesseractacademy.com

Code MIT. Documentation CC BY 4.0.
