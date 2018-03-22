# Primary Classes

Primary classes represent the ontology core. They are followed by helper classes (Type Classes and Property Classes).

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

* sctap:workGroupType
    - e.g. latinLit
    - e.g. greekLit
    - e.g. medievalLatinLit
    - e.g. sentencesCommentaries
    - (these values themselves themselves should be instances of a WorkGroupType class and they should specify available children workGroup types or work types)
    - (see below "workGroupType" class)
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
* role:editor
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

## Repository properties
* sctap:repoUrl
* sctap:hasVersion
    - value of hasVersion should correspond to a version resource for each tagged moment in the source history (i.e. each release)
* sctap:current
    - value of current should point to version which is the top of the source history tree, i.e. the most recent release.
* sctap:isRepoOf
    - Value should be point to Expression with the structureType=item

## Document properties
* sctap:validatedBy
    - should specify the XML schema this xml document was validated against
* sctap:hasXML
    - url to raw xml file

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

## MaterialObject properties

* sctap:materialType =   
    - folio
    - quire
    - codex
* sctap:hasSurface
    - should list every surface contained within this material. So for any given Material object of the type Folio, there should be two "hasSurface" properties
* sctap:hasCanvas
* dc:hasPart
    - can contain surface
    - or another materialObject part
* level
    - codex should be level 1, generally quire would be level 2, etc.

# Type Classes

The above classes (Primary Classes) represent privileged classes that constitute the Ontology core. Type classes are helper classes used in further defining instances of Primary Classes. The goal of type classes is facilitate "favoring objection composition over class inheritance" (See Design Patterns, p. 20)

# Type

## global properties
* rdf:type=Type
* dc:title
    - e.g. "Work Group Type", "Work Type", "Expression Type"
* dc:description

## Type properties
* sctap:typeType
    - WorkGroup
    - Work
    - Expression
    - Manifestation
    - Item

# WorkGroupType

## global properties
* rdf:type=WorkGroupType
* dc:title
    - e.g. "LatinLit", "GreekLit, "MedievalLit", "SentencesCommentary", "EnglishNovels"
* dc:description

## WorkGroupType properties

* sctap:availableWorkGroupType
    - e.g. where WorkGroupType is a workGroup for Medieval Commentaries available workGroupTypes might include:
        + BiblicalCommentaries
        + SentencesCommentaries
* sctap:availableWorkType
    - e.g. where WorkGroupType is a work group for Sentences Commentary available workTypes might include:
        + Reportatio
        + Ordinatio
        + Abbreviatio
* sctap:availableExpressionType
    - a workGroup could specific a list of expression Types available for all descendant workGroups and works
    - e.g. where WorkGroupType is Sentences Commentary available Expression types would be the following
        + commentarius
        + librum
        + distinctio
        + articulus
        + dubium
        + prologus
        + generic
        + paragraph
        + (an expressionType should be created for each of these specified types)
            * these types can further specify other available sub expressionTypes but these sub ExpressionType should take a level property where the value is 2 or greater, so that we know which ExpressionType determines and specifies the other. See below "ExpressionType"
                - e.g. librum -> expressionType=librum1, librum2, librum3, librum4
                - e.g. distinctio = librum1-distinctio1, librum2-distinctio3, etc

# WorkType

## global properties

* rdf: type=WorkType
* dc:title
* dc:description

## WorkType properties

* sctap:availableExpressionType
    - A workType could add other availableExpressionTypes besides those inherited from the WorkGroup Type.

# ExpressionType

## global properties

* rdf:type=ExpressionType
* dc:title
    - e.g. Distinctio
* dc:description

## ExpressionType properties

* sctap:level
    - 1, 2, 3, 4
* sctap:availableExpressionType
    - an ExpressionType can specify sub ExpressionTypes. All expressionTypes not listed as an avaiableExpression type by a workGroupType or workType must have a level 2 or greater ExpressionType. In other words, workGropuTypes and workTypes should only list level 1 ExpressionTypes as available.
    - for example, the ExpressionType "Distinctio" might could have the availableExpresssionTypes. Only Expression with the expressionType "distinctio" could also have the expressionType="librum1-distinctio1"
        + librum1-distinctio1
            * level2
        + librum2-distinctio 15
            * level=2
    - another example for English Novels could be the level 1 expressionType Chapter. Chapters could specify sub ExpresionTypes
        + FirstChapter
        + LastChapter
        + MiddleChapter
        + Chapter2
    - These further specifications are useful to the study of sub genres.
        + What if a person wanted to study the differences in opening chapters among all novels in the English Novel Work group?
        + They could only do this if we had a way to query all expression with the expressionType Chapter that also have the expressionType "Chapter1"

# ManifestationType

## global properties

* rdf:type=ManifestationType
* dc:title
* dc:description

## ManifestationType properties

* sctap:availableManifestType
    - MsWitness
    - Incunabula
    - EarlyModernPrinting
    - ModernPrinting
    - BornDigitalEdition

# ItemType

## global properties

* rdf:type=ItemType
* dc:title
* dc:description

## ItemType properties

* rdf:availableItemType
    - material
    - transcription

# Property Classes

Property class should categorize and organize available properties used in defining Primary and Type Classes

## global properties

* rdf:type
* dc:title
* dc:description

## availableType properties

* sctap:availableWorkType
* sctap:availableWorkGroupType
* sctap:availableExpressionType
* sctap:availableManifestationType
* sctap:availableItemType

## type properties  

* sctap:workGroupType
* sctap:workType
* sctap:expressionType
* sctap:manifestationType
* sctap:transcriptionType
* sctap:materialObjectType
* sctap:typeType
* sctap:structureType

## part properties

* dc:hasPart
* dc:isPartOf
* sctap:level

## linkingProperties

* sctap:hasManifestation
* sctap:isManifestationOf
* sctap:hasTranscription
* sctap:isTranscriptionOf
* sctap:hasMaterialObject
* sctap:isMaterialObjectOf
* sctap:hasDocument

##transcription properties

* sctap:hasXML
* sctap:hasHTML
* sctap:hasPlainText
* sctap:hasJSON

##publicationInfo properties

* role:author
* role:editor
* sctap:creationDate
* sctap:version
* sctap:status
* sctap:citation

## stuctureType properties

* sctap:isPartOfTopLevelExpression
* sctap:hasStructureItem
* sctap:isPartOfStructureItem
* sctap:hasStructureBlock
* sctap:isPartOfStructureBlock
* sctap:hasStructureElement
