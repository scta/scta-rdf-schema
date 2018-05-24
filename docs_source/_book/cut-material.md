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
* sctap:isMemberOf
* sctap:level

## linkingProperties

* sctap:hasManifestation
* sctap:isManifestationOf
* sctap:hasCanonicalManifestation
* sctap:hasTranscription
* sctap:isTranscriptionOf
* sctap:hasCanonicalTranscription
* sctap:hasMaterialObject
* sctap:isMaterialObjectOf
* sctap:isPartOfTopLevelExpression

##transcription properties

* sctap:hasXML
* sctap:hasHTML
* sctap:plaintext
* sctap:hasJSON
* sctap:hasDocument

##publicationInfo properties

* role:author
* role:editor
* sctap:creationDate
* sctap:version
* sctap:status
* sctap:citation (not used)
* sctap:longTitle

## stuctureType properties

* sctap:isPartOfTopLevelExpression
* sctap:isPartOfTopLevelManifestation
* sctap:isPartOfTopLevelTranscription
* sctap:hasStructureItem
* sctap:isPartOfStructureItem
* sctap:hasStructureBlock
* sctap:isPartOfStructureBlock
* sctap:hasStructureElement

* sctap:next
* sctap:previous
* sctap:sectionOrderNumber
* sctap:totalOrderNumber
* ldp:inbox
* sctap:shortId

* hasSlug
  - used in manifestion level (not sure if this is needed)
* sctap:ipfsHash

* sctap:hasCanonicalCodexItem
* sctap:hasCodexItem
* sctap:hasSurface
* sctap:canvasPagedType
* sctap:hasOfficialManifest
* sctap:isCodexItemOf

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



## WorkType properties

* sctap:availableExpressionType
    - A workType could add other availableExpressionTypes besides those inherited from the WorkGroup Type.
    - for example a work, like

* sctap:level
  - 1, 2, 3, 4
* sctap:availableExpressionType
  - an ExpressionType can specify sub ExpressionTypes. All expressionTypes not listed as an avaiableExpression type by a workGroupType or workType must have a level 2 or greater ExpressionType. In other words, workGroupTypes and workTypes should only list level 1 ExpressionTypes as available.
  - for example, the ExpressionType "Distinctio" might have the availableExpresssionTypes. Only Expression with the expressionType "distinctio" could also have the expressionType="librum1-distinctio1"
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
