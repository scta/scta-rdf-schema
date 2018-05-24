# Property List

# A

* sctap:availableExpressionType
  - Domain
    - sctar:expressionType
  - Range
    - sctar:expressionType
* sctap:availableManifestationType
  - Domain
    - sctar:expressionType
  - Range
    - sctar:expressionType

* role:author
  - Domain
    - sctar:expression
    - sctar:manifestation
    - sctar:transcription
  - Range
    - sctar:person
# B

# C

* sctap:canvasPagedType
* sctap:citation (not used)
* sctap:creationDate
  - I'm not sure this is being used

* role:contributor
  - Domain
    - sctar:expression
    - sctar:manifestation
    - sctar:transcription
  - Range
    - sctar:person
  - Note
    - range could be changed to blank node which then indicates a sctar:person plus other kinds of information like the nature and scope of the contribution etc.

# D

* dc:description
  - Domain (object type)
    - global
  - Range (predicate type)
    - "string"

# E

* role:editor
  - Domain
    - sctar:manifestation
  - Range
    - sctar:person


* sctap:expressionType
  - Domain
    - sctar:expression
  - Range
    - sctar:expressionType

# H


* sctap:hasStructureBlock
  - Domain
    - sctar:expression (where structureType = structureItem or structureDivision)
      - Range
        - sctar:expression (where structureType=structureBlock)
    - sctar:manifestation (where structureType = structureItem or structureDivision)
      - Range
        - sctar:manifestation (where structureType=structureBlock)
    - sctar:transcription (where structureType = structureItem or structureDivision)
      - Range
        - sctar:transcription (where structureType=structureBlock)
  - Range
    - sctar:expression (where structureType=structureBlock)
    - sctar:manifestation (where structureType=structureBlock)
    - sctar:transcription (where structureType=structureBlock)


* sctap:hasStructureDivision
  - Domain
    - sctar:expression (where structureType = structureItem or structureDivision)
      - Range
        - sctar:expression (where structureType=structureDivision)
    - sctar:manifestation (where structureType = structureItem or structureDivision)
      - Range
        - sctar:manifestation (where structureType=structureDivision)
    - sctar:transcription (where structureType = structureItem or structureDivision)
      - Range
        - sctar:transcription (where structureType=structureDivision)
  - Range
    - sctar:expression (where structureType=structureDivision)
    - sctar:manifestation (where structureType=structureDivision)
    - sctar:transcription (where structureType=structureDivision)


* sctap:hasStructureElement
  - Domain
    - sctar:expression (where structureType = structureBlock)
      - Range
        - sctar:expression (where structureType=structureElement)
    - sctar:manifestation (where structureType = structureBlock)
      - Range
        - sctar:manifestation (where structureType=structureElement)
    - sctar:transcription (where structureType = structureBlock)
      - Range
        - sctar:transcription (where structureType=structureElement)
  - Range
    - sctar:expression (where structureType=structureElement)
    - sctar:manifestation (where structureType=structureElement)
    - sctar:transcription (where structureType=structureElement)


* sctap:hasStructureItem
  - Domain
    - sctar:expression (where structureType = structureCollection)
      - Range
        - sctar:expression (where structureType=structureItem)
    - sctar:manifestation (where structureType = structureCollection)
      - Range
        - sctar:manifestation (where structureType=structureItem)
    - sctar:transcription (where structureType = structureCollection)
      - Range
        - sctar:transcription (where structureType=structureItem)
  - Range
    - sctar:expression (where structureType=structureItem)
    - sctar:manifestation (where structureType=structureItem)
    - sctar:transcription (where structureType=structureItem)



* sctap:hasXML
* sctap:hasHTML
* sctap:hasJSON
* sctap:hasDocument

* sctap:hasCanonicalCodexItem
* sctap:hasCodexItem
* sctap:hasSurface
* sctap:hasOfficialManifest


* dc:hasPart
  - Domain
    - scta:workGroup
      - Range
        - sctar:workGroup
        - sctar:work
        - sctar:expression
    - scta:work
      - Range
        - sctar:expression
    - scta:expression (where structureType != structureBlock)
      - Range
        - sctar:expression
    - scta:manifestation (where structureType != structureBlock)
      - Range
        - sctar:manifestation
    - scta:transcription (where structureType != structureBlock)
      - Range
        - sctar:transcription
  - Range
    - sctar:workGroup
    - sctar:work
    - sctar:expression
    - scta:manifestation
    - scta:transcription

* sctap:hasManifestation
  - Domain
    - sctar:expression
  - Range
    - sctar:manifestation
* sctap:hasCanonicalManifestation
  - Domain
    - sctar:expression
  - Range
    - sctar:manifestation
* sctap:hasTranscription
  - Domain
    - sctar:manifestation
  - Range
    - sctar:transcription
* sctap:hasCanonicalTranscription
  - Domain
    - sctar:manifestation
  - Range
    - sctar:transcription

* hasSlug
  - used in manifestation level (not sure if this is needed)
  - hasn't this be replaced by shortId

# I

* ldp:inbox
  - Domain
    - Global
  - Range
    - url


* sctap:ipfsHash
  - Domain
    - sctar:transcription
  - Range
    - "string"


* sctap:isCodexItemOf
  - Domain
    - sctar:codexItem
  - Range
    - sctar:codex


* sctap:isManifestationOf
  - Domain
    - sctar:manifestation
  - Range
    - sctar:expression


* sctap:isMemberOf
  - Domain
    - sctar:workGroup (where level !=1)
      - Range
        - sctar:workGroup
    - sctar:work
      - Range
        - sctar:workGroup
    - sctar:expression (where level != 1)
      - Range
        - sctar:expression (where level != 1)
    - sctar:expression (where level = 1)
      - Range
        - sctar:work
        - sctar:workGroup
    - sctar:manifestation (where level != 1)
      - Range:
        - sctar:manifestation
    - sctar:transcription (where level != 1)
      - Range:
        - sctar:transcription
  - Range
    - sctar:workGroup
    - sctar:work
    - sctar:expression
    - sctar:manifestation
    - sctar:transcription
  - Notes
    - Note that manifestations and transcriptions are not allowed to be members of workGroups or works. If one wants to find all manifestations in a work group, they should ask for all expression that are members of a work group and then find all manifestations of those expressions
    - toplevel expressions are allowed to be a member of workgroups and works because they are both level 1 of expression hierarchy and lowest level of the group hierarchy. (workGroup->work->expression, and topLevelCollection->collection-item-div-block-element)


* sctap:isTranscriptionOf
  - Domain
    - sctar:transcription
  - Range
    - sctar:transcription


* sctap:isPartOfStructureBlock
  - Domain
    - sctar:expression (where structureType=structureElement)
      - Range
        - sctar:expression (where structureType=structureBlock)
    - sctar:manifestation (where structureType=structureElement)
      - Range
        - sctar:manifestation (where structureType=structureBlock)
    - sctar:transcription (where structureType=structureElement)
      - Range
        - sctar:transcription (where structureType=structureBlock)
  - Range
    - sctar:expression (where structureType=structureBlock)
    - sctar:manifestation (where structureType=structureBlock)
    - sctar:transcription (where structureType=structureBlock)
  - Note
    - this seem redundant with dc:isPart of unless structureElement is not always a direct child of structureBlock. Perhaps if a quote has another quote as part then this would be needed. But right now all structureElements are reduced to a flat list that are the direct children of structureBlock, so dc:isPartOf would always give the same result as isPartOfStructureBlock
  - Candidate for Revision
    - Proposed changes
      - Keep but document that this is identical to dc:hasPart at this level
      - or Delete


* sctap:isPartOfStructureItem
  - Domain
    - sctar:expression (where structureType=structureBlock or structureElement)
      - Range
        - sctar:expression (where structureType=structureItem)
    - sctar:manifestation (where structureType=structureBlock or structureElement)
      - Range
        - sctar:manifestation (where structureType=structureItem)
    - sctar:transcription (where structureType=structureBlock or structureElement)
      - Range
        - sctar:transcription (where structureType=structureItem)
  - Range
    - sctar:expression (where structureType=structureItem)
    - sctar:manifestation (where structureType=structureItem)
    - sctar:transcription (where structureType=structureItem)


* sctap:isPartOfTopLevelExpression
  - Domain
    - sctar:expression
  - Range
    - sctar:expression (where structureType=structureCollection and level=1)
  - Notes
    - this a short cut property, any query could get here by asking for an expression that is level 1. But this property gives a direct reference to the topLevelExpression by pointing to the resource. Finding the same resource requires a two step query: Find an expression that the current node is a memberOf then give me the expression that has a level one
    - the designation of "expression" also seems necessary since it is will be assumed that we are looking for a top level expression when the subject of the property is an expression
  - Candidate for Revision
    - Proposed Change
      - sctap:isPartOfTopLevelCollection, where range (expression, manifestation, transcription) is determined by the domain.


* sctap:isPartOfTopLevelManifestation
  - Domain
    - sctar:manifestation
  - Range
    - sctar:manifestation (where structureType=structureCollection and level=1)
  - Notes
    - see Notes for sctap:isPartOfTopLevelExpression
  - Candidate for Revision
    - Proposed Change
      - sctap:isPartOfTopLevelCollection


* sctap:isPartOfTopLevelTranscription
  - Domain
    - sctar:transcription
  - Range
    - sctar:transcription (where structureType=structureCollection and level=1)
  - Notes
    - see Notes for sctap:isPartOfTopLevelExpression
  - Candidate for Revision
    - Proposed Change
      - sctap:isPartOfTopLevelCollection



* dc:isPartOf
  - Domain
    - scta:workGroup (where workGroup != top level work group is not a top level workGroup)
      - Range
        - sctar:workGroup
        - sctar:work
        - sctar:expression
    - scta:work
      - Range
        - sctar:expression
    - scta:expression
      - Range
        - sctar:expression
    - scta:manifestation
      - Range
        - sctar:manifestation
    - scta:transcription
      - Range
        - sctar:transcription
  - Range
    - sctar:workGroup
    - sctar:work
    - sctar:expression
    - scta:manifestation
    - scta:transcription

# L

* sctap:level
  - Domain
    - sctar:expression
    - sctar:manifestation
    - sctar:transcription
    - (could also apply to workGroup and work, but work would be the lowest level, as topLevelExpress has level 1)
  - Range
    - integer

* sctap:longTitle


# M

* sctap:materialObjectType (not sure this is used)
* sctap:manifestationType
  - Domain (object type)
    - sctar:manifestation
  - Range (predicate type)
    - sctar:manifestationType


# N

* sctap:next
 - Domain
  - sctar:expression (structureType != topLevel Collection )
    - Range
      - sctar:expression
  - sctar:manifestation (structureType != topLevel Collection )
    - Range
      - sctar:manifestation
  - sctar:transcription (structureType != topLevel Collection )
    - Range
      - sctar:transcription
  - sctar:surface
    - Range
      - sctar:surface
  - Notes/Questions
    - would something with structureType=structureElement takes next and previous???

# O

# P

* sctap:plaintext
  - Domain  
    - sctar:transcription
  - Range
    - url

# S

* sctap:status
  currently being at expression level
  but I think it should apply only to transcription level
  - Domain
    - sctar:transcription
  - Range
    - "string": "draft"
    - "string": "private-draft"
    - "string": "reviewed"
    - "string": "not-started"

* sctap:structureType
  - Domain (object type)
    - sctar:expression
    - sctar:manifestation
    - sctar:transcription
    - (role of work is unclear)


* sctap:shortId
  * sctap:sectionOrderNumber
  - Domain
    - sctar:expression
  - Range
    - integer
  - Notes
    - Should the domain only be expression. I think so. If so, should "level" be restricted to expression as well.

# T

* sctap:totalOrderNumber
  - Domain
    - sctar:expression
  - Range
    - integer
  - Notes
    - Should the domain only be expression. I think so. If so, should "level" be restricted to expression as well.


* dc:title
  - Domain (object type)
    - global
  - Range (predicate type)
    - "string"


* sctap:transcriptionType
  - Domain (object type)
    - sctar:transcription
  - Range (predicate type)
    - sctar:transcriptionType
    - or Literal, "critical", "translation", "diplomatic"


* rdf:type
  - Domain (object type)
    - global
  - Range (predicate type)
    - "string"


# U

# V

  * sctap:version
    - Domain
      - sctar:transcription
    - Range
      - "string"

# W
