#Primary Classes

Primary classes represent the ontology core. They are followed by helper classes (Type Classes and Property Classes).

#workGroup

##global properties
* type=workGroup
* label
    - e.g. Latin Literature
* description


## workGroup properties
* workGroupType 
    - e.g. latinLit 
    - e.g. greekLit
    - e.g. medievalLatinLit
    - e.g. sentencesCommentaries
    - (these values themselves themselves should be instances of a WorkGroupType class and they should specify available children workGroup types or work types)
    - (see below "workGroupType" class)
* isPartOf
    - if the workGroup is not a levelType=topLevel and level=1 it must specify its parent part. This must be another workGroup.
* hasPart
    - a work group can take either another workGroup or a work as its direct part (or child)
* hasWork
    - every work group should list every Work contained within it no matter what level
* levelType
    - topLevel
        + if a workGroup is not contained by another work group it should be listed as "topLevel"
    - part
        + if a workGroup is contained by another workGrop it's levelType should be "part"
* level
    - 1,2,3
* citation


#Work

##global properties
* type=work
* label
    - e.g. Adam Wodeham Ordinatio, Moby Dick
* description


## work type properties
* author
* workType (available workTypes should be determined by the available workTypes specified by the WorkGroupType)
    - e.g. Novel
    - e.g. SentencesCommentary
    - e.g. BiblicalCommentary
* citation

#Expression

##global properties
* type=expression
* label
    - e.g. Wodeham Oxford Ordinatio
* description


##expression type properties
* expressionType
    - e.g. available expressionTypes should be enumerated in the WorkType or WorkGroupType class
    - e.g. a Work within the WorkGroup where workGroupType="SentencesCommentaries" the value of expressionType given here must be one of the values of the availableExpressionType listed in that workGroupType. See WorkGroupType and WorkType below.
        + Commentarius
        + Librum
        + Distinctio
        + Quaestio
        + Articulus
        + Dubium
* levelType
    - topLevel
    - part
    - fragment 
* structureType
    - structureCollection
    - sturctureItem
    - sturctureBlock
    - sturctureElement
* hasManifestation
* citation

##expressionType->levelType=topLevel
* level=1
    - topLevel expression must be level one
* hasPart
    - object of hasPart must be another expression

##expressionType=part
* level
* hasPart
    - value of hasPart must be another Expression
* isPartOf
    - value of isPartOf must be another Expression

##structureType=structureCollection properties
* hasStructureItem

##structureType=structureItem properties
* isPartOfStructureCollection
    - value can only be applied if Expression is not a topLevel Expression.Value must point to a topLevel Expression
* hasBlock
* gitRepo
* status
* generalEditor
* currentEdition
* canonicalManifestation
* canonicalTranscription

##structureType=structureDivision properties
* isPartOfStructureItem    
* hasStructureBlock

##structureType=structureBlock properties
* isPartOfStructureItem    
* hasStructureElement

##structureType=structureElement properties
* isPartOfStructureBlock    


#Manifestation

##global properties
* type=manifestation
* label
    - e.g. Sorbonne ms. 253
* description
    - e.g. Sorbonne witness to paragraph 4
    - e.g. Sorbonne witness to quote `<quote id>`

##manifestation properties
* manfestationType
    - manuscript
    - incunabula
    - earlyEdition
    - modernEdition
    - bornDigitalEdition
* hasItem (not to be confused with structureType=Item above)
* isManifestationOf
    - Value must be an Expression
* citation  
* page
    - should point to page/side object
        + e.g. f. 15r or p. 16

#Item
(item class should not be confused with structureType=item)
##global properties
* type=item
* label
    - Sorbonne ms. 253 material witness
    - Sorbonne ms. 253 transcriptions
* description

##Item properties 

* itemType
    - materialObject
    - transcription 
        + i.e. digital transcription of manifestation

##itemType=transcription
* xml
* plainText
* json
* editor
* status
* edition
* hasDocument, isPartOfDocument, isComposedFromDocument
    - only transcriptions of manifestations of expressions with the structureType=item should take a hasDocument property, expressions with a structureType=division should take a isPartOfDocumentProperty and expressions with a structureType=collection should take two or more isComposedFromDocument properties.
* hasZone
    - should be point to the Zones on which this transcription falls.
    - this is mostl easily applied at the paragraph level. A paragraph that extends over from one page to the next should have two zones, each zone demarcating the coordinate regions on the respective surface falls

##itemType=materialObject
* hasNote (not to be confused with Annotation in IIIF/Open Annotation)
    - should point to a Note class, note classes by definition are material inscriptions on a particular material item and therefore should only be attached at the ItemLevel

#Note 
##global properties
* type=Note
* label
* description

##Note Propertes
* isOnItem
    - should point to the materiObject item for the manifestation of the expression that the note most closely refers to. 
    - Thus, a note close connected to a paragraph, should apply to the materialObject item for the manifestation of the expression of the paragraph this notation is most closely connected. If the note is more general and does not apply to a particular pargraph, it should be connected a point higher in the expression hierarchy (i.e. OHCO).
* hasZone
    - should point to the Zone coordinate region on which the notation falls

#Repository

##global properties
* type=repository
* label = "Git Hub Development Repository"
* description

##Repository properties
* repoUrl
* version 
    - value of version should correspond to a version resource for each tagged moment in the source history (i.e. each release)
* current
    - value of current should point to version which is the top of the source history tree, i.e. the most recent release.
* isRepoOf
    - Value should be point to Expression with the structureType=item

#Version

##global properties
* type=version
* label = Version 1.0.0
* description

## Version properties
* hasDocument
    - should point to document resource (i.e. each xml file contained in the repository at this point in the commit history)

#Document

##global properties
* type=Document
* label = TEI Transcription of Sorbonne ms 253
* description

## Document properties
* validatedBy
    - should specify the XML schema this xml document was validated against
* xml 
    - url to raw xml file

#Zone

##global properties
* type=Zone
* label = Zone for 
* description

##Zone properties

* isOnRegion
    - marginRight
    - marginCenter
    - marginLeft
    - marginBottom
    - marginTop
    - column
* isOnSurface    
* isOnCanvas
* order
    - default=1, where there is more than one zone for a given resource, (for example a paragraph that crosses from one column to the next) the zone should be given a order number so that the zones can be ordered correctly.

#Region
##global properties
* type=region
* label
    - marginRight
    - marginLeft

##Region Properties
* hasZone
* isOnSurface
* isOnCanvas
* regionType
    - margin
    - column
    - etc.

#Surface
##global properties
* type=suface
* label
    - f. 1r

##surface properties
* hasZone
* hasRegion
* isOnCanvas
* isPartOf
    - surface should be the smallest unit in the material hierarchy, contained by folio, quire, codex, etc. In modern text, it should normally correspond to a page.

#Material
##global properties
* type=material
* label
    - f. 1r

##Material properties
* materialType =   
    - folio
    - quire
    - codex
* hasSurface
    - should list every surface contained within this material. So for any given Material object of the type Folio, there should be two "hasSurface" properties
* hasCanvas
* hasPart
    - can contain surface
    - or another material part
* levelType
    - topLevel
        + topLevel material cannot be contained by another material class. (i.e. the codex should not be contained by anything else.)
    - part
* level
    - codex should be level 1, generally quire would be level 2

#Type Classes
The above classes (Primary Classes) represent privileged classes that constitute the Ontology core. Type classes are helper classes used in further defining instances of Primary Classes. The goal of type classes is facilitate "favoring objection composition over class inheritance" (See Design Patterns, p. 20) 

#Type

##global properties
* type=Type
* label 
    - e.g. "Work Group Type", "Work Type", "Expression Type"
* description

##Type properties
* typeType
    - WorkGroup
    - Work
    - Expression
    - Manifestation
    - Item

#WorkGroupType

##global properties
* type=WorkGroupType
* label 
    - e.g. "LatinLit", "GreekLit, "MedievalLit", "SentencesCommentary", "EnglishNovels"
* description

##WorkGroupType properties
* availableWorkGroupType
    - e.g. where WorkGroupType is a workGroup for Medieval Commentaries available workGroupTypes might include:
        + BiblicalCommentaries
        + SentencesCommentaries
* availableWorkType
    - e.g. where WorkGroupType is a work group for Sentences Commentary available workTypes might include:
        + Reportatio
        + Ordinatio
        + Abbreviatio
* availableExpressionType
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

#WorkType

##global properties
* type=WorkType
* label 
* description

##WorkType properties
* type=WorkType
* availableExpressionType
    - A workType could add other availableExpressionTypes besides those inherited from the WorkGroup Type.

#ExpressionType

##global properties
* type=ExpressionType
* label 
    - e.g. Distinctio
* description

##ExpressionType properties
* level
    - 1, 2, 3, 4
* availableExpressionType
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

#ManifestationType

##global properties
* type=ManifestationType
* label 
* description

##ManifestationType properties
* availableManifestType
    - MsWitness
    - Incunabula
    - EarlyModernPrinting
    - ModernPrinting
    - BornDigitalEdition

#ItemType

##global properties
* type=ItemType
* label 
* description

##ItemType properties
* availableItemType
    - material
    - transcription

# Property Classes
Property class should categorize and organize available properties used in defining Primary and Type Classes

## global properties
* type
* label
* description

##availableType properties
* availableWorkType
* availableWorkGroupType
* availableExpressionType
* availableManifestationType
* availableItemType

##type properties  
* workGroupType
* workType
* expressionType
* manifestationType
* itemType
* typeType
* itemType
* levelType
* structureType

## part properties
* hasPart
* isPartOf
* level

## linkingProperties
* hasManifestation
* isManifestationOf
* hasItem
* isItemOf
* hasDocument

##Item->type->transcription properties
* xml
* html
* plaintext
* json

##publicationInfo properties
* author
* editor
* creationDate
* version
* status
* citatin

## stuctureType properties
* hasStructureItem
* IsPartOfStructureCollection (value must point to topLevel Expression)
* isPartOfStructureItem
* hasStructureBlock
* hasStructureElement
* isPartOfStructureBlock




