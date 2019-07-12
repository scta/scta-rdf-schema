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

A reference should look similar:

`<ref xml:id="quotation-id" target="id-of-passaging-containing-referenced-text">Lorum ipsum</ref>`

If it is desirable to add an additional editorial reference, the `quote` or `ref` should be wrapped in `cit` tag with child `bibl` like so:

`<cit>
  <ref xml:id="ref-id" target="id-of-passaging-containing-referenced-text">Lorum ipsum</ref>
  <bibl>Modern reference</bibl>
</cit>`

In theory, the target/source id should be de-referenceable and the Expression sctap:longTitle should be able to be retrieved, along with a desired Manifestation citation information, rendering the need to provide a manual bibliographic reference absolute.

This possibility is illustrated here: https://scta.github.io/scta-citation-creator/?resourceid=http://scta.info/resource/pgb1q1-cadanl

Nevertheless, it is a desired practice to provide a manual reference as a fall back and to allow the TEI file to stand on its own.

(Note: Current practice allows allows for the `target` or `source` attribute to be replaced by an `@ana` attribute whose value is "#" + shortID of a `canonicalQuotation`. The canonicalQuotation in turn should take a `source` attribute pointing to the passage containing the canonical quotation. (Please see [structureElementSeg](structureElementSeg.md) proposal for possible changes to the use of "canonical quotation").)

(Note further: The inconsistency between using the shortID or the full RDF URL id is confusing. It could be adopted as an official policy that any id with a "#" should be followed by the shortID and therefore is always resolvable to "http://scta.info/resource" + shortId. Or, the official policy could decide exclusively in favor of one pattern over the other, and the rejected pattern should be depreciated and discouraged. Or in place of the hash, we could begin to demand the prefix `sctar:` where sctar: is headed to the XML file name space as `xmlns:sctar="http://scta.info/resource/"`.

# Example 2: Quotation with an associated reference.

A slightly more complex example, but still very common occurrence, is to have a quotation with an associated reference within the text.

This should be marked up as follows:

```xml
<ref xml:id="ref-id" corresp="#quotation-id">Lorum ipsum</ref>
<cit>
  <ref xml:id="quotation-id" target="id-of-passaging-containing-referenced-text">Lorum ipsum</ref>
  <bibl>Modern reference</bibl>
</cit>
```

The above markup is currently converted at build time to the following set of relationships.

A `ref` becomes an expression with a `structureType=structureElementType` and further takes a property called `elementType` designating it as an `structureElementReference`.

A `quote` becomes an expression with a `structureType=structureElementType` and further takes a property called `elementType` designating it as an `structureElementQuotation`.

When a `ref` does not have an `@corresp` attribute it is considered to be an independent reference (that is, not associated with a quotation). In such a cases, it should have an `@ana` or `@target` property when the target is known and a resource id exists.

In such cases, the value of the `@ana` attribute becomes the value of the `sctap:isInstanceOf` property confirming that this reference is an instance of a canonical quote or passage.

Alternatively, the value of the `@target` becomes the value of the `sctap:source` identifying the target as the source passage of the quotation in question.

But when, as is the case here, the `ref` has a `@corresp`, the quotation to which the reference is attached is the `isInstanceOf` or `source` and the `ref` takes the `isReferenceTo` property.  

In such cases, when the `ref` has a `@corresp`, it should not have an `@ana` or `@target` because, again, it is the quotation that has the `source` or `isInstanceOf`. The `source` for the ref can always be found by beginning at the quotation (as an object) and then following the property `isReferenceTo` to the (subject) `ref` in question.

# Example 3: Quotation with two associated reference.

However, there is a complication. Sometimes we have quotation with two associated references:
one to the quotation source and a second to the place where the quotation was found by the quoting author.

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

In this case the quote is from Gregory and ref mentioning Gregory
should have an `@corresp` but no `@ana` or `@target` because it will inherit this from the quotation.
But the Magister ref applies to the quote. However, in a way, it is also an independent
reference to the place where Lombard quotes Gregory.

In this case, it seem likes the ref should have an `@corresp` and a `@target`.

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

Example
  * http://sctalab.lombardpress.org/#/text?resourceid=http://scta.info/resource/pgb1q19-d1e234/critical/transcription
    * Another good example of this kind of quotation connection occurs in Gracilis q. 19. Here Gracilis quotes a Augustine,   and then provides a reference to Lombard and says this quote is in Lombard. The quote in Lombard is much longer than the passage cited byGgracilis. The passage quoted by Gracilis is contained with the passage quoted by Lombard.
    * Currently I've set the reference source to the paragraph containing the quotation. But it would be more precise to set the target of the reference to the quotation id of the quotation in Lombard.

## Example 4

A similar but slightly different example is the case in which there is only one reference,
but the reference is to the place where the quotation is found by the author,
but not to the source of the quotation.

Most references to canon law are like this.

In the following example, Bonaventure the author,
uses a ref to Lombard as the reference for a quote by Gregory and Gregory who remains unnamed.

```xml
Item,
  <cit>
    <quote xml:id="id-of-quote" source="id-of-passage-containing-gregor-text">
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

The above encoding basically means, here is a quote from Gregory the Great. But here is a ref for this quote that points to the proxy source for the quote.

In this case, the graph build would and assign the reference a `source` property and an `isReferenceTo` property.

This would allow a query like the following.

"Show me all the quotes of Augustine that are quotations referenced through or attributed to Lombard.""

The query would check the `@source` attribute of the `quote` to find the quotes, and then check for references for this quote using the `corresp` or `isReferenceTo` property, and then check to see if a `source` property is present and if source is a passage from Lombard.

This also mean that a ref with a `@corresp` attribute could be wrapped in a `cit` tag with its own `bibl`. The `bibl` for the quote should a default modern reference for the actual quote. The `ref` that directly corresponds should take no `cit` or `bibl` because it is covered by the `quote` but the second `ref` with an `@source` and `@corresp` can take its own `bibl` which is the modern citation patter for the reference.

Example
  * http://sctalab.lombardpress.org/#/text?resourceid=http://scta.info/resource/pgb1q13-d1e876/critical/transcription
    * Here Gracilis provides a quotation of Apuleius through Augustine's City of God.


## Example 5 (gloss)

What about a case in which almost every word is a quote, in the case of a word by word commentary

One very common example  can be found in Bonaventure. (See Bonaventure bb-l4d49p2s1a2q1(alias))

Here he quotes a bible verse and then begin explaining the meaning of each word. In the traditional print edition, the bible verse is marked as italic, and then each re-quoted word is marked in italic.s

```xml
<p>Respondeo : Dicendum quod numerus istarum dotium sive qualitatum perficientium corpus accipitur ex Sapientiae 3, 7 :
  <cit>
    <quote ana="#sap3_7">Fulgebunt iusti et tanquam scintillae in arundineto discurrent</quote>
  </cit> :
  in
  fulgore <!-- visually encoded in italis -->
  claritas ;
  in iustitia <!-- visually encoded in italis -->
  impassibilitas, quia iustitia est perpetua et immortalis ;
  in scintillae <!-- visually encoded in italis -->
  nomine subtilitas ; in discursu agilitas.
</p>
```
One option might be to mark each word with "quote" but that doesn't feel right to me because he's not actually re-quoting
and the words are not even precisely found in the quote as restated. Instead they seem to function as anchors to refer back to smaller part of the quote he wants to clarify.

I think `<mentioned>` might be most appropriate, but it might be nice to give `<mentioned>` an `@corresp` that points back to the quotation (namely the quotation `@xml:id` that it is a part of. Perhaps `<mentioned>` could take an `@type=gloss` and if so, why not use `<quote>` even for these individual words, but qualified with the `@type` gloss.

But the case might be more complicated if the author starts quoting and commenting on single words without ever providing a full quote beforehand. Shoud these be `<quote>` or `<mentioned>`

## Example 6 (lemma)

Related to the use of a quote qualifier like `gloss` we have also played around with options where the quotation is given a `@type=lemma`

In this case, we seem to mean that a quotation is given, but its function is different than regular quotes.

```xml
<p xml:id="pgb1q1-cadanl">
  <cit>
    <quote xml:id="pg-b1q1-Qd1e3733" source="http://scta.info/resource/pll1prol-cadzdd" type="lemma">
      Cupientes aliquid de penuria</quote>
    <bibl>
      <ref target="http://scta.info/resource/pll1prol-cadzdd">
        <name>Lombardus</name>,
        <title>Sent.</title>
        I, prol., [n. 1]
        (Brady I, 3, ll. 1).
      </ref>
    </bibl>
  </cit>
  etc.
  Istud est prooemium
  libri
  <title ref="#lombardsententia">Sententiarum</title>
  qui liber dividitur in prooemium et tractatum.
  Secunda incipit ibi
  <cit>
    <quote xml:id="pg-b1q1-Qd1e3742" source="http://scta.info/resource/pll1d1c1-vanspv" type="lemma">
      veteris ac novae legis
    </quote>
    <bibl>
      <ref target="http://scta.info/resource/pll1d1c1-vanspv">
        <name>Lombardus</name>,
        <title>Sent.</title>
        I, d. 1, c. 1, [n. 1]
        (Brady I, 55, ll. 5).
      </ref>
    </bibl>
  </cit>.
</p>
```

## Example 7 (paraphrase/allusion)

There are also cases where an author seems to be quoting. "ille dicit: " but what follows cannot be regarded as a direct quote but a summary or paraphrase of what is being said. Nevertheless the paraphrased content is clearly attributed to the author of the quote. It has a clear ending and beginning. It would be nice to be able to capture these quotations and compare them against direct quotes.

So far we generally use the type "paraphrase" to describe this.

But what if the use of the quote is much weaker. For example it doesn't start with "ille dicit" but just sort of flows into a sentence that is more or less what another author says. It might be tempting to mark this as well, `@type=allusion` could be a possible way to mark this.

 But at certain point, the editor is no longer marking what appears in the text, but is really making an editorial comment about their recognition, as editor, that what they a reading here sounds a lot like what someone else says. In this case, I think these assertions should be collected elsewhere, perhaps in the "info.xml" file.

## Example 8

What about a case where the quotation is used primarily as a point of reference or incipit in a reference, such as in a pointer to a quote from the "glossa ordinaria".

Examples:

* There are many examples of this in Bonaventure.
* See http://sctalab.lombardpress.org/#/text?resourceid=http://scta.info/resource/bb-d1e12582-d1e163/critical/transcription
* At present I encode the incipit from the bible verse as its in quote in order to make sure the bible verse can be found, then I record the quote from the glossa as a second quote.
* But I still debate whether it is correct to treat the bible verse as a quote rather than as a part of the reference.
  * Bonaventure is really trying to say: At the point in the bible that says..., the Glossa says...

## Example 9

We also have cases where a single ref could be functioning as a reference for two quotations. In this case the `@corresp` attributes needs to be able to take two values separated by white space.

See the example in Gracilis pg-b1q8

```xml
<p xml:id="pgb1q8-d1e572">
  Tertio,
  <cit>
    <quote xml:id="pg-b1q8-Qd1e575">
      quidquid de ratione formali est principium productivum
      alicuius secundum suam rationem formalem
      in quocumque illud est erit a se principium productivum alicuius
    </quote>
    <bibl>
      <name>Scotus</name>,
      <title>Ordinatio</title>
      I, d. 2, pars 2, q. 4
      (Vatican II, 263, ll. 15-18).
      <!-- dbcheck -->
    </bibl>
  </cit>.
  Sed
  <cit>
    <quote xml:id="pg-b1q8-Qd1e592">
      intellectus habens obiectum sibi
      praesens ex ratione formali est principium notitiae genitae
    </quote>
    <bibl>
      <name>Scotus</name>,
      <title>Ordinatio</title>
      I, d. 2, pars 2, q. 4
      (Vatican II, 259, ll. 10-12).
      <!-- dbcheck -->
    </bibl>
  </cit>,
  ergo cum talis sit in Deo,
  sequitur quod ibi sit notitia genita.
  Haec est ratio
  <ref xml:id="pg-b1q8-Rd1e4255" corresp="#pg-b1q8-Qd1e575 #pg-b1q8-Qd1e592">
    <!-- first ref example I have with two correspon quotes; seems like it could be a common use case. -->
    <name ref="#Scotus">Scoti</name>,
    distinctione secunda primi
  </ref>
</p>
```
Example:

* See: http://sctalab.lombardpress.org/#/text?resourceid=http://scta.info/resource/pgb1q8-d1e572/critical/transcription
