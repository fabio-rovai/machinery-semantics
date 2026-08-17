# Korean machine builders have until 14 January 2027, and the open standards they will rely on are not as clean as they look

The EU Machinery Regulation (EU) 2023/1230 applies from 14 January 2027. On that date Directive 2006/42/EC is repealed. We verified both dates against the EUR-Lex primary text, Article 54, because a number of vendor summaries circulating online give 20 January and are simply wrong. Parts of the Regulation already bite: Articles 26 to 42 have applied since 14 January 2024.

For Korean machine tool and equipment exporters, from Doosan and Hyundai WIA down to component suppliers, this converts documentation into a data problem. Instructions, declarations and technical documentation move toward machine-readable form, and the semantic carriers the industry has settled on are the Asset Administration Shell and, for shop floor data, MTConnect.

We audited both. Here is what we found, including a number of ours that did not survive scrutiny.

## MTConnect publishes the same standard twice, and the two copies disagree

MTConnect publishes each version as both an XSD and a JSON Schema. That makes them checkable against each other, which is unusual and to the standard's credit. We compared versions 2.0, 2.1 and 2.2.

The JSON Schema defines AlarmStateEnum with the values INSTANT, ACTIVE and CLEARED. The XSD defines AlarmStateType with only ACTIVE and CLEARED. The string INSTANT appears nowhere in either XSD document. The discrepancy is present identically in all three released versions.

The practical consequence is narrow but real. A JSON-side implementation may legitimately emit INSTANT, and an XSD-validating consumer will reject the message. If your integration crosses that boundary, you have a conformance question that neither document answers.

## The number we had to throw away

Our first pass reported 111 divergences between the two representations. That number was wrong and we are publishing it because the correction is more instructive than the finding.

Of the 111, 105 were the UNAVAILABLE sentinel appearing in the XSD enumerations but not the JSON ones. The JSON Schema handles unavailability structurally, through an isUnavailable flag and a separate value constraint, so this is a deliberate design difference rather than a defect. A further three were an artefact of our own name matching, which collapsed two genuinely distinct XSD types, CoordinateSystemEnumType and CoordinateSystemTypeEnumType, onto a single key across a file boundary.

That leaves one real defect, appearing in three releases. We would rather publish one verified finding than a hundred impressive ones.

## The Asset Administration Shell is genuinely open, and a common assumption about it is false

The IDTA publishes 54 submodel templates under CC BY 4.0, including the ones a machine builder exporting to the EU will actually need: Digital Nameplate, Functional Safety, Handover Documentation, Intelligent Information for Use, Carbon Footprint and Digital Product Passport.

We expected to find that these open templates depend on paywalled semantic dictionaries, which would have meant every implementer needs a commercial ECLASS or IEC CDD subscription to resolve the meaning of the fields. That expectation was wrong. Across the published templates we counted 1,529 references to IDTA's own IRIs against only 18 ECLASS and 9 IEC CDD references. The templates are overwhelmingly self-referential. Anyone telling you that compliance requires buying a dictionary subscription should be asked to show their counts.

## What this means for a Korean exporter

Three things follow. Your compliance evidence is becoming machine-readable, so it inherits the defects of the schemas you build on. The schemas are open, which means you can audit them yourself rather than trusting a vendor's assurance. And where two published representations of the same standard disagree, the disagreement is yours to resolve at integration time unless somebody raises it upstream first.

We have raised the MTConnect discrepancy with the standard's maintainers.

## How we checked

Every number above was computed two independent ways and we treat a disagreement between methods as a defect in our own work. For the ontology material we used our open-source Open Ontologies engine alongside rdflib. The MTConnect comparison is a short script over the published XSD and JSON Schema files, both of which are freely available. One caution for anyone repeating it: the MTConnect XSDs use single-quoted XML attributes, and a regular expression written for double quotes returns zero matches and looks like a clean result.

## Working with us

We offer bounded first engagements. For a machine builder, that is a fixed-scope readiness assessment against the 14 January 2027 deadline, covering which submodel templates your product documentation actually needs, where your current data falls short, and a reproducible conformance script you keep.

Kampakis and Co Ltd, trading as The Tesseract Academy. fabio@thetesseractacademy.com
