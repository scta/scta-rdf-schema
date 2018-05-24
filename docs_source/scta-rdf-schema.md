# Resource Types

## Resource

All resources inherit the global properties of the generic resource type

* rdf:type
  - http://scta.info/resource/workGroup
  - http://scta.info/resource/work
  - http://scta.info/resource/expression
  - http://scta.info/resource/manifestation
  - http://scta.info/resource/transcription
  - etc.
* dc:title
  - e.g. Wodeham Oxford Reportatio
  - e.g. Wodheam Abbreviatio
  - Beethoven's 9th 1st Redaction
  - Beethoven's 9th 2nd Redaction
  - Moby Dick the Novel
  - Moby Dick the Screen Play
* dc:description
* sctap:shortId
* ldp:inbox


## workGroup

* dcterms:isPartOf
    - if the workGroup is not level=1 it must specify its parent part. This must be another workGroup.
* dcterms:hasPart
    - a work group can take either another workGroup or a work as its direct part (or child)
* sctap:hasExpression
    - every work group should list every expression contained within it, no matter what level
* sctap:level
    - 1,2,3
* sctap:citation
    - a string offering a best practice example of how this should be cited

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

## Text Resources Chart

TODO: prose summary of how x and y axis relate to individual resources and available properties.

| | [Expression](#expression) | [Manifestation](#manifestation) | [Transcription](#transcription) |
| ---------- | ---------- | ------------- | ------------- |
| [TopLevelStructureCollection](#toplevelstructurecollection) | [TopLevelCollectionExpression](toplevelcollectionexpression) | [TopLevelCollectionManifestation](#toplevelcollectionmanifestation) |  [TopLevelCollectionTranscription](#toplevelcollectiontranscription) |
| [StructureCollection](#structurecollection) | [ExpressionCollection](#expressioncollection) | [ManifestationCollection](#manifestationcollection) | [TranscriptionCollection](#transcriptioncollection) |
| [StructureItem](#structureitem) | [ExpressionItem](#expressionitem) | [ManifestationItem](#manifestationitem) | [TranscriptionItem](#transcriptionitem) |
| [StructureDivision](#structuredivision) | [ExpressionDivision](#expressiondivision) | [ManifestationDivision](#manifestationdivision) | [TranscriptionDivision](#transcriptiondivision) |
| [StructureBlock](#structureblock) | [ExpressionBlock](#expressionblock) | [ManifestationBlock](#manifestationblock) | [ItemBlock](#itemblock) |
| [StructureElement](#structureelement) | [ExpressionElement](#expressionelement) | [ManifestationElement](#manifestationelement) | [ItemElement](#itemelement) |

## Global Text Resource Properties

* role:author
* role:editor (is this global? I think maybe only a manifestation can have an editor)
* role:contributor
* sctap:citation

## Expression

* rdf:type=http://scta.info/resource/expression
* sctap:status
* sctap:expressionType
    - e.g. available expressionTypes should be enumerated in the WorkType or WorkGroupType class
    - e.g. a Work within the WorkGroup where workGroupType="SentencesCommentaries" the value of expressionType given here must be one of the values of the availableExpressionType listed in that workGroupType. See WorkGroupType and WorkType below.
* sctap:hasManifestation
* sctap:hasCanonicalManifestation
* sctap:isPartOfTopLevelExpression <!-- I would like to see this change just isPartOfTopLevel and become a property of structures -->
    - all non top level Expressions should point to the top level Expression of which they are a part.

## Manifestation

* sctap:manfestationType
    - manuscript
    - incunabula
    - earlyEdition
    - modernEdition
    - bornDigitalEdition
* sctap:hasCanonicalTranscription
* sctap:hasTranscription
* sctap:isManifestationOf
    - Value must be an Expression
* sctap:hasMaterialObject
* sctap:surface
    - should point to each page/side object that this manifestation falls on.
        + e.g. f. 15r or p. 16
        + see Surface class below.
* sctap:startsOnSurface
* sctap:endsOnSurface

## Transcription

* sctap:transcriptionType
    - diplomatic
    - critical
    - translation
* sctap:isTranscriptionOf
* sctap:hasXML
* sctap:hasDocument
* sctap:plaintext
* sctap:status

* sctap:hasZone
    - should point to the Zones on which this transcription falls.
    - this is most easily applied at the paragraph level. A paragraph that extends over from one page to the next should have two zones, each zone demarcating the coordinate regions on the respective surface falls.
    - (TODO: note I'm not yet sure if it logically more precise for a Manifestation to take a hasZone property rather than a Transcription resource. Or perhaps both.)

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


## codex

* sctap:hasCanonicalCodexItem
* hasCanonicalCodexItem
* sctap:hasSurface

## codexItem

* canvasPagedType
* hasOfficialManifest
* isCodexItemOf

## person

* hasInstance
* personType
* owl:sameAs (this should be global)

---

Below Needs Substantial Review and Revision

---

Note on reference and quotation Properties (needs review)

The current working assumption is that `refs` get an `isInstanceOf` property
when they do NOT have a `@corresp` but do have an an `@ana` or an `@target`

but when, they have a `@corresp`, the quote is the `instanceOf` and the ref takes the
`isReferenceTo` property.  

Likewise, when they have a `@corresp` they should not have an `@ana` or `@source`
for again the quote is the instanceOf and the ref the source can be found by following
the `isReferenceTo` property to the quotation in question.

However, there is a complication.

Sometimes we have something like this:

>Item, <quote>omnia dona sunt omnibus commu-
nia</quote>, sicut dicit <ref>Gregorius</ref> et <ref>Magister in littera</ref>;
sed denominatio angelicorum ordinum flt a donis
gratuitis: ergo oinnia nomina eoruin debent esse
communia, quemadmoduin et dona.

In this case the quote is from Gregory and the gregory ref should have an `@corresp` but no `@ana` or `@target` because it will inherit this from the quote. But the Magister ref applies to the quote, but in a way is a reference to the place where Lombard quotes Gregory.
In this case, it seem like the ref should have an `@corresp` and a `@target`

Similar example:

>Item, <quote>omnia dona sunt omnibus commu-
nia</quote>, sicut dicit <ref>Magister in littera</ref>;
sed denominatio angelicorum ordinum flt a donis
gratuitis: ergo oinnia nomina eoruin debent esse
communia, quemadmoduin et dona.

In this case Bonavanture the author, uses a ref to Lombard as the reference for a quote by Gregory and Gregory remains unnamed.

For this reason, a `<ref>` should be allowed to take a `@target` property at the same time that is has an `@corresp`.

This basically means, here is a quote, without its own source. But here is a ref for this quote that points to the proxy source for the quote.

This would allow a query like the following. Show me all the quotes of Augustine that are quotations referenced through or attributed to Lombard.

The query would the `@source` attribute of the `quote` to find the quotes, and then check for references for this quote using the `corresp` or `isReferenceTo` property, and then check to see if the source for this reference is from Lombard.

This also mean that a ref with a corresp attribute could be wrapped in a `cit` tag with its own `bibl`. The `bibl` for the quote should a default modern reference for the actual quote. The `ref` that directly corresponds should take no `cit` or `bibl` because it is covered by the `quote` but the second `ref` with an `@source` and `@corresp` can take its own `bibl` which is the modern citation patter for the reference.

See Gracilis pg-b1q13 for a quotation of Apuleius through Augustine's city of God for another good example.



## Note

* sctap:isOnMaterialObject
    - should point to the materiObject for the Manifestation of the Expression that the note most closely refers to.
    - Thus, a note close connected to a paragraph, should apply to the materialObject  for the Manifestation of the Expression of the paragraph this notation is most closely connected. If the note is more general and does not apply to a particular pargraph, it should be connected a point higher in the Expression hierarchy (i.e. OHCO).
* hasZone
    - should point to the Zone abstract coordinate region on which the notation falls.


## Zone properties

* sctap:isOnRegion
    - marginRight
    - marginCenter
    - marginLeft
    - marginBottom
    - marginTop
    - column
* sctap:isOnSurface    
* sctap:isOnCanvas
* sctap:order
    - default=1, where there is more than one zone for a given resource, (for example a paragraph that crosses from one column to the next) the zone should be given a order number so that the zones can be ordered correctly.

## Region Properties

* sctap:hasZone
* sctap:isOnSurface
* sctap:isOnCanvas
* sctap:regionType
    - margin
    - column
    - etc.

## Surface properties

* sctap:hasZone
* sctap:hasRegion
* sctap:isOnCanvas
* sctap:isPartOf
    - surface should be the smallest unit in the material hierarchy, contained by folio, quire, codex, etc. In modern text, it should normally correspond to a page.
