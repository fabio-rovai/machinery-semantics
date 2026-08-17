# Taiwan's machine tool cluster has until 14 January 2027, and the standards it will build on contain a defect we can show you

The EU Machinery Regulation (EU) 2023/1230 applies from 14 January 2027, the same day Directive 2006/42/EC is repealed. We verified both against the EUR-Lex primary text at Article 54, because several vendor summaries in circulation give 20 January and are wrong. Articles 26 to 42 have already applied since 14 January 2024.

Taiwan is one of the world's largest machine tool exporters, and the Taichung cluster, from Hiwin and Delta through Tongtai and Victor Taichung, sells heavily into Europe. For those firms this Regulation turns technical documentation into a data engineering problem. The semantic carriers the industry has converged on are the Asset Administration Shell for product documentation and MTConnect for shop floor data.

We audited both, and we are publishing a number of our own that failed verification alongside the one that survived.

## MTConnect publishes each version twice, and the copies disagree

MTConnect releases every version as both an XSD and a JSON Schema, which makes the two mutually checkable. We compared 2.0, 2.1 and 2.2.

The JSON Schema defines AlarmStateEnum with INSTANT, ACTIVE and CLEARED. The XSD defines AlarmStateType with only ACTIVE and CLEARED. The string INSTANT does not appear anywhere in either XSD. The discrepancy is identical across all three released versions.

The consequence is narrow and real. A JSON-side implementation may legitimately emit INSTANT and an XSD-validating consumer will reject it. If your integration crosses that boundary, neither document tells you which side is correct.

## The finding we retracted

Our first pass reported 111 divergences between the two representations. That number was wrong, and the correction matters more than the original claim.

Of the 111, 105 were the UNAVAILABLE sentinel present in the XSD enumerations but absent from the JSON ones. The JSON Schema handles unavailability structurally through an isUnavailable flag and a separate value constraint, so this is a deliberate design difference and not a defect. Another three were an artefact of our own name normalisation, which collapsed two genuinely distinct XSD types, CoordinateSystemEnumType and CoordinateSystemTypeEnumType, onto one key across a file boundary.

One real defect remains, present in three releases. We would rather publish one verified finding than a hundred impressive ones.

## The Asset Administration Shell is open, and a widespread assumption about it is false

The IDTA publishes 54 submodel templates under CC BY 4.0, including those a Taiwanese exporter will need: Digital Nameplate, Functional Safety, Handover Documentation, Intelligent Information for Use, Carbon Footprint and Digital Product Passport.

We expected these open templates to depend on paywalled semantic dictionaries, which would have meant compliance quietly requires a commercial ECLASS or IEC CDD subscription. That expectation was wrong. Across the published templates we counted 1,529 references to IDTA's own IRIs against 18 ECLASS and 9 IEC CDD references. The templates are overwhelmingly self-referential. If a vendor tells you compliance requires buying a dictionary, ask them for their counts.

## What this means in Taichung

Your compliance evidence is becoming machine-readable, so it inherits the defects of the schemas beneath it. Those schemas are open, so you can audit them yourself instead of trusting an assurance. And where two published representations of one standard disagree, resolving that disagreement falls to you at integration time unless someone raises it upstream first.

We have raised the MTConnect discrepancy with the standard's maintainers.

## How we checked

Every number here was computed two independent ways, and disagreement between methods is treated as a defect in our own work rather than a finding. For ontology material we used our open-source Open Ontologies engine alongside rdflib. The MTConnect comparison is a short script over the published files, which are freely available. One warning for anyone repeating it: the MTConnect XSDs use single-quoted XML attributes, so a regular expression written for double quotes silently returns zero matches and looks like a clean bill of health.

## Working with us

We offer bounded first engagements. For a machine builder, that is a fixed-scope readiness assessment against the 14 January 2027 deadline, covering which submodel templates your documentation actually requires, where your current data falls short, and a reproducible conformance script that stays with you.

Kampakis and Co Ltd, trading as The Tesseract Academy. fabio@thetesseractacademy.com
