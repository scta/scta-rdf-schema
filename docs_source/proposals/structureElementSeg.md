## Recommend Change from `Quotation` (CanonicalQuotation) resource type to `structureElementSeg`

In this proposal I described a recommended shift away using a resource type called "Quotation" (meant to identify canonical quotations to which quotation instances can be mapped) and adopting instead a new `structureElementType` called `structureElementSeg` that would be identified through standOff markup rather than inline markup to allow for cross-nesting text Segments and mapping across manifestations and transcriptions. structureElementSeg would allow us to refer to granular text parts, and structureElementSeg could be nested within each other.

Let's consider an example.

Suppose we have a quotation of part of Genesis 1:1  "In the beginning **God created the heavens and the earth**". (Call this Instance A). In this case, we might want to keep track of two different things. We might want to be able keep track of every quotation that only has the second half "God created the heavens and the earth" (Seg1) and we might also want to keep track of every quotation instance that includes even a part of Genesis 1:1. If Genesis 1:1 was treated as a structureType=structureBlock, the fragment "God created the heavens and the earth" (Seg1) could be structureElementSeg that has an `isPartOf` and `isMemberOf` property that points to the id for Genesis 1:1. Now imagine another quotation (Instance B) cites "In the beginning God created" (Seg2) crossing over the bounds of the uncited and cited portion of the passage in the previous quotation instance. Here we could create a new structureElementSeg for "In the beginning God created" which is `isPartOf` and `isMemberOf` the larger verse. Finally, another quotation (Instance C) might quote simply quote `God created` (Seg3). This could be listed as `isPartOf` of Both Instance A and Instance B and `isMemberOf` Genesis 1.

The benefit of moving away from a category of quotation or canonical quotation is that this pattern can be expanded for other kinds of references to granular text fragments, such as references to the "major premise" or the "minor premise" or the "antecedent" or the "consequent".

Keeping with the example text we can imagine the desired result as something like the following:

```ttl

sctar:bibl-gen1_1
  rdf:type sctar:expression ;
  sctap:structurType sctap:structureBlock ;
  sctap:hasText "In the beginning God created the heavens and the earth" ;
  sctap:isPartOf sctar:bibl-gen ;
  sctap:isMemberOf sctar:bibl-gen ;
  sctap:isPartOfStructureBlock sctar:bibl-gen1_1 ;
  // all other expression properties allowed

sctar:seg1
  rdf:type sctar:expression ;
  sctap:hasText "God created the heavens and the earth" ;
  sctap:isPartOf sctar:bibl-gen1_1 ;
  sctap:isMemberOf sctar:bibl-gen1_1 ;
  sctap:isPartOfStructureBlock sctar:bibl-gen1_1 ;
  // all other expression properties allowed

sctar:seg2
  rdf:type sctar:expression ;
  sctap:hasText "In the beginning God created" ;
  sctap:isPartOf sctar:bibl-gen1_1 ;
  sctap:isMemberOf sctar:bibl-gen1_1 ;
  sctap:isPartOfStructureBlock sctar:bibl-gen1_1 ;
  // all other expression properties allowed

sctar:seg3
  rdf:type sctar:expression ;
  sctap:hasText "God created" ;
  sctap:isPartOf sctar:seg1 ;
  sctap:isPartOf sctar:seg2 ;
  sctap:isMemberOf sctar:seg1 ;
  sctap:isMemberOf sctar:seg2 ;
  sctap:isMemberOf sctar:bibl-gen1_1 ;
  sctap:isPartOfStructureBlock sctar:bibl-gen1_1 ;
  // all other expression properties allowed

```

A quoting these segments would like the following:

```xml
<p>
  This is a commentary and now I'm commenting the passage in Genesis that says
  <cit>
    <quote xml:id="instanceA" source="sctar:seg1">God created the heavens and the earth</quote>
    <bibl>Genesis 1:1</bibl>
  </cit>
  but now I want discusses the earlier part of this verse that bleeds into what was discussed above
  <cit>
    <quote xml:id="instanceB" source="sctar:seg2">In the beginning God created</quote>
    <bibl>Genesis 1:1</bibl>
  </cit>
  and finally I want to say something about the words
  <cit>
    <quote xml:id="instanceC" source="sctar:seg3">God created</quote>
    <bibl>Genesis 1:1</bibl>
  </cit>
  which appears in both of the other two quotes
</p>

```

This markup converted into the above graph would allow very granular searches, you could still ask for all quotations instances of genesis 1:1, but you could also ask for more granular divisions. The advantage of this is less clear in the case of bible verses since the block structure is very short. The advantage is more clearly seen, for instances, in a text from Augustine whose paragraph/block structure might be very long, and we would like to cite individual sentences or passages within that paragraph and keep track of all instances quoting or referencing these precise passages.

Further, as citation across instances is never exact, there is no need to fret if a quotation instance does not exactly fit existing structureElementSeg. A new structureElementSeg can be made and then situated in relation to other existing structureElementSegs, or the one can always revert to citing the parent structureBlock.

The difficult lies in how to create the structureElementSeg in a stand off way, that can also map to all of the manifestations and transcriptions of these text fragment.

We need a standoff markup file. Currently, I've been using an info.xml file embedded within each structureItem directory.

Up until the info.xml file has been used to assert relations between other resources that do not have a clear element hook in the TEI. What would be a little unusual about creating this resources, is that an editor would actual be creating an expression resource for a text other than the text to which the info.xml is associated.

But it doesn't matter too much where the record is made, and if there were a predictable pattern to creating ids, two people making graphs about a Seg with the same id would be seamlessly merged.
Alternatively a separate repository could be created for Segs, and a web form could be created to coordinate the creation of Segs.

Another reason for moving from a Quotation (canonicalQuotation) resource to a structureElementSeg is that we have the added benefit of treating this as a type expression.

Quotations that are being cited should be treated in the same manner as any other expression, and since we already have the structure for organizing an expression, we should be treating it as an expression. The only difference is its structureType.

This means, the creation of Seg should also include assertions (or a way to construct assertions) about available manifestations and transcriptions and transcription should include xpath to the token range of the cited passage.

Something like the following might provide enough information to produce the required information

```xml
<seg xml:id="sctar:seg1">
  <manifestations>
    <manifestation xml:id="sctar:seg1/sorb193">
      <transcriptions>
        <transcription xml:id="sctar:seg1/sorb193/transcription">
        <ranges>
          <range>
            <isPartOfStructureBlock>sctar:bibl-gen1_1/sorb193/transcription</isPartOfStructureBlock>
            <tokenStart>4</tokenStart>
            <tokenEnd>end</tokenEnd>
          </range>
          <!-- if the quote continued across to another block and second range could be added -->
        </transcription>
      </manifestation>
  </manifestations>
</seg>
```

Using the structureBlockId the transcription id of the block plus the token position
can be used to request the text range from the TEI. (Some difficulties need to be addressed here about nested token positions. For example we would need to exclude text from bibl, app/rdgs, and notes, but flatten all the text token within the paragraph including quotes, refs, lem, title, name, etc.)

If other manifestations and transcriptions have different orders, tokenStart and tokenEnd properties would have to be added for each.

While this might work, it seems tedious and laborious, but it might be able to automated or made more efficient with some kind of webform.
