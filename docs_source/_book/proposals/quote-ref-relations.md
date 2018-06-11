# Quotation Reference Passage Relations

## Introduction

The activity of quoting and referencing is one of the most dominant activity of the scholastic tradition. As such it highly desirable to be able trace these connections with detail and precision in a bi-directional way. This is a complicated task and therefore needs close attention.

We need to give close attention to the ultimate web of relations that we want to generate from text markup and accordingly we need to describe in detail the nature of this markup, and how it maps to the desired resultant network model.

Below we will work our way through the simplest examples to the most complex examples.

# Example 1: Simple Quotation or Reference

The simplest case is a stand alone quotation or reference.

In this case an author will quote or reference a section of text from another text.

In the case of a quotation this should be marked up as follows:

`<quote xml:id="quotation-id" source="id-of-passaging-containing-referenced-text">Lorum ipsum</quote>`

A reference should look similar

`<ref xml:id="quotation-id" target="id-of-passaging-containing-referenced-text">Lorum ipsum</ref>`

If is a desired to an additional editorial reference the `quote` or `ref` should be wrapped in `cit` tag with child `bibl` like so:

`<cit>
  <ref xml:id="ref-id" target="id-of-passaging-containing-referenced-text">Lorum ipsum</ref>
  <bibl>Modern reference</bibl>
</cit>`

In theory, the target/source id should be de-referencable and the Expression sctap:longTitle should be able to be retrieved, along with a desired Manifestation citation information, rendering the need to provide a manual bibliographic reference absolute.

Nevertheless, it is desired practice to provide a manual reference as a fall back and to allow the TEI file to stand on its own.

Current practice allows allows for the `target` or `source` attribute be replaced by an `ana` attribute whose value is "#" + shortID of a `canonicalQuotation`. The canonicalQuotation in turn should take a `source` attribute pointing to the passage containing the canonical quotation. (Please see [structureElementSeg](structureElementSeg.md) proposal for possible changes to the use of "canonical quotation").

Note: The inconsistency between using the shortID or the full RDF URL id is confusing. It could be adopted as official policy that any id with a "#" should be followed by the shortID and therefore is always resolvable to "http://scta.info/resource" + shortId. Or official policy should decide exclusively in favor or one patter over the other, and the rejected pattern should be depreciated and discouraged. Or in place of the hash, we could begin to demand the prefix `sctar:` where sctar: is headed to the XML file name space as `xmlns:sctar="http://scta.info/resource/"`.


# Example 2: Quotation with an associated reference.

A slightly more complex example, but still very common occurence is to have a quotation with a associated reference with in the text.

This should be marked up as follows:

```xml
<ref xml:id="ref-id" corresp="#quotation-id">Lorum ipsum</ref>
<cit>
  <ref xml:id="quotation-id" target="id-of-passaging-containing-referenced-text">Lorum ipsum</ref>
  <bibl>Modern reference</bibl>
</cit>
```

The above markup is currently converted at build to following set of relationships.

A `ref` becomes an expression with a structureType=structureElementType and further takes a property called `elementType` designating it as an structureElementReference.

A `quote` becomes an expression with a structureType=structureElementType and further takes a property called `elementType` designating it as an structureElementQuotation.

When a ref does not have an `corresp` attribute is considered to be an independent reference (that is not associated with a quotation). In such a case it should have an `@ana` or `@target` property when the target is known and a resource id exists.

In such cases, the value of the `@ana` or `@target` property becomes the value of the `sctap:isInstance` property confirming that this reference is an instance of a canonical quote or passage.

But when, as is the case here, they have a `@corresp`, the quotation to which the reference is attached is the `instanceOf` and the ref takes the `isReferenceTo` property.  

In such cases, when the `ref` has a `@corresp` it should not have an `@ana` or `@target` because, again, it is the quotation that is the instanceOf. The for this instance can always be found by beginning at the `isInstanceOf` quotation and then following the property `isReferenceTo` to the `ref` in question.

# Example 3: Quotation with two associated reference.

However, there is a complication. Sometimes we have quotation with two associated references: one to the quotation source and second to the place where the quotation was found.

For example:

```xml

<p>Item,
  <cit>
    <quote>omnia dona sunt omnibus communia</quote>
    <bibl>Gregory the Great, Moralia, xxx</bibl>
  </cit>,
  sicut dicit
  <ref>Gregorius</ref>
  et
  <ref>Magister in littera</ref>;
  sed denominatio angelicorum ordinum flt a donis
  gratuitis: ergo oinnia nomina eoruin debent esse
  communia, quemadmoduin et dona.
</p>

```

In this case the quote is from Gregory and the Gregory ref should have an `@corresp` but no `@ana` or `@target` because it will inherit this from the quote.
But the Magister ref applies to the quote, but in a way is also an independent reference to the place where Lombard quotes Gregory.
In this case, it seem like the ref should have an `@corresp` and a `@target`.

The fully encoded example would then look like the following:

```xml

<p>Item,
  <cit>
    <quote xml:id="quote-id" source="id-of-source-passage">omnia dona sunt omnibus communia</quote>
    <bibl>Gregory the Great, Moralia, xxx</bibl>
  </cit>,
  sicut dicit
  <ref xml:id="ref-id" corresp="#quote-id">Gregorius</ref>
  et
  <ref xml:id="ref-id" target="id-of-passage-where-lombard-quotes-gregory">Magister in littera</ref>;
  sed denominatio angelicorum ordinum flt a donis
  gratuitis: ergo oinnia nomina eoruin debent esse
  communia, quemadmoduin et dona.
</p>

```

## Example 4

A similar but slightly different example is the case in which there is only one reference, but the reference is to the place where the quotation is found by the other, but not to the source of the quotation.

In this case Bonavanture the author, uses a ref to Lombard as the reference for a quote by Gregory and Gregory remains unnamed.

```xml
Item,
  <cit>
    <quote xml:id="id-of-quote">
      omnia dona sunt omnibus communia
    </quote>
  </cit>,
  sicut dicit
  <ref xml:id="id-of-ref" corresp="#id-of-quote" source="id-of-passage-where-lombard-quotes-this-quote">
    Magister in littera
  </ref>;
  sed denominatio angelicorum ordinum flt a donis
  gratuitis: ergo oinnia nomina eoruin debent esse
  communia, quemadmoduin et dona.

```

Given this example, a `<ref>` should be allowed to take a `@target` property at the same time that is has an `@corresp`. The combination of the `@target` and `@corresp` is the only thing that differentiates this case from example 2.

The above encoding basically means, here is a quote, without its own source. But here is a ref for this quote that points to the proxy source for the quote.

In this case, the graph build would assign the reference both an `isInstanceOf` property and `isReferenceTo` property.

This would allow a query like the following. Show me all the quotes of Augustine that are quotations referenced through or attributed to Lombard.

The query would the `@source` attribute of the `quote` (isIstance) to find the quotes, and then check for references for this quote using the `corresp` or `isReferenceTo` property, and then check to see if the source (isInstanceOf) for this reference is from Lombard.

This also mean that a ref with a `@corresp` attribute could be wrapped in a `cit` tag with its own `bibl`. The `bibl` for the quote should a default modern reference for the actual quote. The `ref` that directly corresponds should take no `cit` or `bibl` because it is covered by the `quote` but the second `ref` with an `@source` and `@corresp` can take its own `bibl` which is the modern citation patter for the reference.

Below are some other good examples of similar patterns.

See Gracilis pg-b1q13 for a quotation of Apuleius through Augustine's city of God for another good example.

Another good example of quotation connections occurs in Gracilis q. 19. Here Gracilis quotes a Augustine, and then provides a reference to lombard and says this quote is in lombard. The quote in lombard is much longer than the passage cited by gracilis. The passage quoted by Gracilis is contained with the passage quoted by Lombard.

Quote source in Gracilis is Augustine, but the reference is to Lombard. Currently I've set the reference to the paragraph containing the quotation. But it would be more precise to set the target of the reference to the quotation id of the quotation in lombard.

The canonical quotation pattern doesn't work to well here, because the quotation in gracilis and in lombard cannot be said to be the instance of the same quotation exemplar. (See Gracilis paragraph: pgb1q19-d1e234)

## Example 5

What about a case in which almost every word is a quote, in the case of a word by word commentary

## Example 6

What about a case where the quotation is used primarily as a point of reference or incipit in a reference, such as in a pointer to a quote from the "glossa ordinaria".
