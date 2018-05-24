# Resource Type Types

The above classes (Primary Classes) represent privileged classes that constitute the Ontology core. Type classes are helper classes used in further defining instances of Primary Classes. The goal of type classes is facilitate "favoring objection composition over class inheritance" (See Design Patterns, p. 20)

# ExpressionType

All expressions have a have a property called sctap:expressionType.
The object of those predicates are of rdf:type=expressionType

## Properties

* sctap:availableExpressionType
  - Range
    - sctar:expressionType
      - e.g.
        - sctar:librum1
        - sctar:da-liber1
        - sctar:librum1-prologus
        - etc
 * dc:isPartOf
  - Range
    - sctar:expressionType
 * dc:hasPart
  - Range
    - sctar:expressionType
 * sctap:isMemberOf
  - Range
    - ssctar:expressionType

# ManifestationType

All manifestations have a have a property called sctap:manifestationType. The object of those predicates are of rdf:type=manifestationType

## Properties

* sctap:availableManifestationType
  - Range
    - sctar:ManifestationType
      - e.g.
        - sctar:MsWitness
        - sctar:Incunabula
        - sctar:EarlyModernPrinting
        - sctar:ModernPrinting
        - sctar:BornDigitalEdition
* dc:isPartOf
 - Range
   - sctar:expressionType
* dc:hasPart
 - Range
   - sctar:expressionType
* sctap:isMemberOf
 - Range
   - sctar:expressionType

# transcriptionType

* sctap:availableTranscriptionType
  - Range
    - sctar:TranscriptionType
      - e.g.
        - sctar:diplomatic
        - sctar:critical
        - sctar:translation
* dc:isPartOf
 - Range
   - sctar:transcriptionType
* dc:hasPart
 - Range
   - sctar:transcriptionType
* sctap:isMemberOf
 - Range
   - sctar:transcriptionType

# PersonType
