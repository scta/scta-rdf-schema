#WorkGroup

##global properties
* type=workGroup
* label
* description

## workGroup type properties
* workGroupType 
    - e.g. latinLit 
    - e.g. greekLit
    - e.g. medievalLatinLit
    - e.g. sentencesCommentaries
    - (these resources themselves should be classes that specify available workGroup types or work types)

#Work

##global properties
* type=work
* label
    - e.g. Adam Wodeham Ordinatio, Moby Dick
* description

## work type properties
* author
* workType (available workTypes should be determined by the WorkGroup)
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
    - topLevel
    - part
    - fragment 
* structureType
    - collection
    - item
    - block
    - element
* hasManifestation

##expressionType=topLevel
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

##structureType=collection properties
* hasItem

##structureType=Item properties
* isPartOfCollection
    - value can only be applied if Expression is not a topLevel Expression.Value must point to a toplevel Expression
* hasBlock
* gitRepo
* status
* generalEditor
* currentEdition
* canonicalManifestation
* canonicalTranscription

##structureType=division properties
* isPartOfItem    
* hasBlock

##structureType=block properties
* isPartOfItem    
* hasElement

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
* hasFRBRItem (not to be confused with structureItem above)
* isManifestationOf
    - Value must be an Expression
* page
    - should point to page/side object
        + e.g. f. 15r or p. 16

#FRBRItem

##global properties
* type=item
* label
    - Sorbonne ms. 253 material witness
    - Sorbonne ms. 253 transcriptions
* description

##FRBRItem properties
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
* validatedBy
    - should specify the XML schema this xml document was validated against
* xml 
    - url to raw xml file
