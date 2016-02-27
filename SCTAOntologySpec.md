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
* hasWorkGroup
    - A work group conta
* hasWork
* levelType
    - topLevel
        + if a workGroup is not contained by another work group it should be listed ast topLevel
    - part
        + if a workGroup is contained by another workGrop it's levelType should be "part"
* level
    - 1,2,3

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

#Expression

##global properties
* type=expression
* label
    - e.g. Wodeham Oxford Ordinatio
* description

##expression type properties
* expressionType
    - e.g. availalbe expressionTypes should be enumerated in the WorkType or WorkGroupType class
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
    - value can only be applied if Expression is not a topLevel Expression.Value must point to a toplevel Expression
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
    - e.g. Sorbonne witness to quote ta-52-yu

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


#Repository

##global properties
* type=repository
* label = "Git Hub Development Repository"
* description

##Repository properites
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

#Type Classes
The above classes (Primary Classes) represent privileged classes that constitute the Ontology core. Type classes are helper classes used in further defining instances of Primary Classes. The goal of type classes is facilitate "favoring objection composition over class inheritance" (See Design Patterns, p. 20) 

#Type

##global properties
* type=Type
* label 
    - e.g. "Work Group Type", "Work Type", Expression Type
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
    - a workGroup could specific a list of expression Types available for all descendent workGroups and works
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
            * these types can further specify other avaiable sub expressionTypes but these sub ExpressionType should take a level property where the value is 2 or greater, so that we know which ExpressionType determines and specifies the other. See below "ExpressionType"
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
* description

##ExpressionType properties
* level
    - 1, 2, 3, 4
* availableExpressionType
    - an ExpressionType can specify sub ExpressionTypes. All expressionTypes not listed as an avaiableExpression type by a workGroupType or workType must have a level 2 or greater ExpressionType. In other words, workGropuTypes and workTypes should only list level 1 ExpressionTypes as available.

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
Property class should categorize and organize available properties used in definiting Primary and Type Classes

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

## stuctureType properties
* hasStructureItem
* IsPartOfStructureCollection (value must point to topLevel Expression)
* isPartOfStructureItem
* hasStructureBlock
* hasStructureElement
* isPartOfStructureBlock




