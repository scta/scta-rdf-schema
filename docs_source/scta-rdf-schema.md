# Resource Types

## Resource

All resources inherit the global properties of the generic resource type

* rdf:type
  - etc.
* dc:title
  - e.g. Wodeham Oxford Reportatio
  - e.g. Wodheam Abbreviatio
  - Beethoven's 9th 1st Redaction
  - Beethoven's 9th 2nd Redaction
  - Moby Dick the Novel
  - Moby Dick the Screen Play
* dc:description
* [sctap:shortId](http://scta.info/property/shortId)
* ldp:inbox


## workGroup

* dcterms:isPartOf
    - if the workGroup is not level=1 it must specify its parent part. This must be another workGroup.
* dcterms:hasPart
    - a work group can take either another workGroup or a work as its direct part (or child)
* [sctap:hasExpression](http://scta.info/property/hasExpression)
    - every work group should list every expression contained within it, no matter what level
* [sctap:level](http://scta.info/property/level)
    - 1,2,3
* sctap:citation
    - a string offering a best practice example of how this should be cited

## Text
  A super class for grouping work with expressions, manifestations, and transcriptions

## Work

* role:author
* dcterms:isPartOf
    - a work should indicate the immediate parent workGroup of which it is a part.
* dcterms:hasPart
    - a Work (unlike a WorkGroup) can take no workGroups or Works as children and thus can only have a top level Expression as its immediate child part.
* sctap:citation
* sctap:hasExpression
* sctap:hasCanonicalExpression
* sctap:level

# textItem

A super class for expressions, manifestations, and transcriptions

## textItem Resources Chart

![View visualization](images/SCTASCHEMA-Viz.png)

| ----- | [Expression](#expression) | [Manifestation](#manifestation) | [Transcription](#transcription) |
| ---------- | ---------- | ------------- | ------------- |
| [TopLevelStructureCollection](#toplevelstructurecollection) | [TopLevelCollectionExpression](toplevelcollectionexpression) | [TopLevelCollectionManifestation](#toplevelcollectionmanifestation) |  [TopLevelCollectionTranscription](#toplevelcollectiontranscription) |
| [StructureCollection](#structurecollection) | [ExpressionCollection](#expressioncollection) | [ManifestationCollection](#manifestationcollection) | [TranscriptionCollection](#transcriptioncollection) |
| [StructureItem](#structureitem) | [ExpressionItem](#expressionitem) | [ManifestationItem](#manifestationitem) | [TranscriptionItem](#transcriptionitem) |
| [StructureDivision](#structuredivision) | [ExpressionDivision](#expressiondivision) | [ManifestationDivision](#manifestationdivision) | [TranscriptionDivision](#transcriptiondivision) |
| [StructureBlock](#structureblock) | [ExpressionBlock](#expressionblock) | [ManifestationBlock](#manifestationblock) | [ItemBlock](#itemblock) |
| [StructureElement](#structureelement) | [ExpressionElement](#expressionelement) | [ManifestationElement](#manifestationelement) | [ItemElement](#itemelement) |

## Global textItem Resource Properties

* role:author
* role:editor (is this global? I think maybe only a manifestation can have an editor)
* role:contributor
* sctap:citation

## Expression

* rdf:type=http://scta.info/resource/expression
* sctap:status
* [sctap:expressionType](http://scta.info/property/expressionType)
    - e.g. available expressionTypes should be enumerated in the WorkType or WorkGroupType class
    - e.g. a Work within the WorkGroup where workGroupType="SentencesCommentaries" the value of expressionType given here must be one of the values of the availableExpressionType listed in that workGroupType. See WorkGroupType and WorkType below.
* [sctap:hasManifestation](http://scta.info/property/hasManifestation)
* [sctap:hasCanonicalManifestation](http://scta.info/property/hasCanonicalManifestation)
* [sctap:isPartOfTopLevelExpression](http://scta.info/property/hasCanonicalManifestation)<!-- I would like to see this change just isPartOfTopLevel and become a property of structures -->
    - all non top level Expressions should point to the top level Expression of which they are a part.

## Manifestation

* [sctap:manfestationType](http://scta.info/property/manifestationType)
    - e.g.
    - manuscript
    - incunabula
    - earlyEdition
    - modernEdition
    - bornDigitalEdition
* sctap:hasCanonicalTranscription
* sctap:hasTranscription
* sctap:isManifestationOf
    - Value must be an Expression
* sctap:isOnSurface (only at item, block, and element level)
* sctap:isOnZone (only block, and element level)

## Transcription

* [sctap:transcriptionType](http://scta.info/property/transcriptionType)
    - e.g.
    - diplomatic
    - critical
    - translation
* sctap:isTranscriptionOf
* sctap:hasXML
* sctap:hasDocument
* sctap:plaintext
* sctap:status
* Version Chain Properties
  - A transcription can forms part of a version chain, allows select transcriptions to be linked together in a version chain.
  - A version chain is not an RDF class, but an idea of transcriptions resources linked together by the following properties.
  * sctap:hasAncestor
    - indicates all previous versions within a version chain. An transcription MAY have many `hasAncestor` properties
  * sctap:hasDescendant
    - indicates all subsequent versions within a version chain. A transcription MAY have many `hasDescendant` properties
  * sctap:hasSuccessor
    - indicates the transcription immediately following this this transcription within the version chain.
    A transcription MAY have ONLY one `hasSuccessor` property
  * sctap:hasPredecessor
    - indicates the transcription immediately preceding this transcription within the version chain. A transcription MAY have ONLY one `hasPredecessor` property
  * sctap:isVersionDefault
    - Within a version chain, a transcription can be identified as the `versionDefault`. The `versionDefault=true` assertion indicates that this transcription that will be associated with a manifestation via the `hasTranscription` property and will be used for extraction purposes in processing. Only one transcription within a version chain should be the object of a manifestation's `hasTranscription` property, while each transcription within the version chain has a `isTranscriptionOf` property.
  * sctap:isHeadTranscription
    - No matter which version is the listed as the `versionDefault` the `isHeadTranscription=true` indicates which transcription stands at the top of the versionChain. the transcription with the `isHeadTranscription=true` will always have the highest `versionOrderNumber` within a series
  * sctap:versionOrderNumber
    - the `versionOrderNumber` indicates the order of a transcription within a version chain. The first transcription will always have the version number "0001" and the head transcription will always have the highest number within the series.
  * sctap:versionNo
    - A version Number 1.0.0 or 1.0.0. Ideally version number will match a git tag or or part of a git tag.
  * sctap:versionLabel
    - A human readable version label designed for display.

---

Note: The following are not classes but critical properties that that textItem classes can take
that effectively divide textItem classes into subclasses

--

## Global Structure properties

* sctap:structureType
    - structureCollection
    - sturctureItem
    - sturctureBlock
    - sturctureElement
* dcterms:isPartOf
    - a top level expression should point to the immediate parent Work (or WorkGroup) that contains the top level expression as a direct child
* dcterms:hasPart
  - the top level expression should list the immediate 2nd level child Expressions
* isPartOfTopLevelStructure
  - alias of isPartOfTopLevelExpression, isPartOfTopLevelManifestation, isPartOfTopLevelTranscription
  - applies at every level accept structureCollection level 1
* sctap:next
    - next should point to the next expression on the same level of the content hierarchy
    - obviously, a top level expression would not a have a next property because there is be definition now sibling of Expression of a top level Expression
* sctap:previous
    - next should point to the next expression on the same level of the content hierarchy
    - again, previous property does not apply to a top level Expression
* sctap:sectionOrderNumber
    - the order sequence position number among sibling Expression parts
* sctap:totalOrderNumber
    - the order sequence position number among all Expression parts on this level.
* sctap:level
    - the level of the expression within the Expression hierarchy

## TopLevelStructureCollection

* sctap:hasStructureItem

## StructureCollection

* sctap:hasStructureItem

## StructureItem

* sctap:hasStructureBlock

## StructureDivision

* sctap:hasStructureBlock

## StructureBlock

* sctap:isPartOfStructureItem
* sctap:hasStructureElement

## StructureElement

* sctap:isPartOfStructureBlock


## Virtual textItem Classes

---

Note: The following are not explicitly classes,
but could be considered sub classes that are the resultant combination of a
textItem class and a structure type

---
### TopLevelCollectionExpression
### ExpressionCollection
### ExpressionItem
### ExpressionBlock
### ExpressionElement
### TopLevelCollectionManifestation
### ManifestationCollection
### ManifestationItem
### ManifestationBlock
### ManifestationElement
### TopLevelCollectionTranscription
### TranscriptionCollection
### TranscriptionItem
### TranscriptionBlock
### TranscriptionElement



## marginalNote

* isPartOfStructureBlock
* isPartOfTopLevelManifestation
* structureType=structureElement
* structureElementType=structureElementMarginalNote
* structureElementText
* isOnSurface
* isOnZone

## person

* hasInstance
* personType
* owl:sameAs (this should be global)

# materialResource

A super class for materialResource subclasses

![View visualization](images/SurfaceVisualization.png)

| MaterialManifestation | MatieralItem |
| ---------- | ---------- |
| [Codex](#codex) | [icodex](#icodex) |
| [Quire](#quire) | [iquire](#iquire) |
| [Folio](#folio) | [ifolio](#ifolio) |
| [Surface](#surface) | [isurface](#isurface) |
| [zone](#zone) | [izone](#izone) |


## codex

* sctap:hasCanonicalCodexItem
* sctap:hasSurface
  - range: sctar:surface
* dc:hasPart
  - range: e.g, sctar:quire

* structureType=structureCollection


## quire

* sctap:hasCanonicalQuireItem
* sctap:hasSurface
  - range: sctar:surface
* dc:hasPart
  - range: e.g sctar:folio, surface
* materialStructureType=materialStructureCollection
* isMemberOf
  - each ancestor of this quire (codex should be listed)

## folio

* sctap:hasCanonicalFolioItem
* sctap:hasSurface
  - range: sctar:surface
* dc:hasPart
  - range: sctar:quire or sctar:codex
* materialStructureType=materialStructureCollection
* isMemberOf
  - each ancestor of this folio (quire, codex should be listed)

## surface

* sctap:isPartOfCodex
* sctap:next
* sctap:previous
* dc:isPartOf
    - e.g. folio, quire, or codex
* dc:hasPart
    - range: directChildren
* sctap:hasZone
  - all descendant zones at any level
* materialStructureType=materialStructureItem
* isMemberOf
  - each ancestor of this surface (folio, quire, codex should be listed)


## zone

* sctap:zoneType
  - marginRight
  - marginCenter
  - marginLeft
  - marginBottom
  - marginTop
  - column
  - line
  - general
* sctap:isPartOfSurface
  - inverse of hasZone
* dc:isPartOf
  - range: sctar:surface, or line is part of column, marginal note could be part of margin
* materialStructureType=materialStructureBlock
* height
* width
* lrx
* lry
* ulx
* uly
* isMemberOf
  - each ancestor of this zone (surface, folio, quire, codex should be listed)

* Note on position/order
  * sctap:order?
    - default=1, where there is more than one zone for a given resource, (for example a paragraph that crosses from one column to the next) the zone should be given a order number so that the zones can be ordered correctly.
    - I think this should be removed and the manifestation pointing to more than one zone should indicate the order using a blank node, e.g hasZone->:bn->zone and order

## icodex

* canvasPagedType
* hasOfficialManifest
* isCodexItemOf
* sctap:hasISurface
  - range: sctar:isurface
* dc:hasPart
  - range: sctar:iquire
* materialStructureType=materialStructureCollection

## iquire

## ifolio

## isurface

## izone
