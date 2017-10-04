# Data sharing: standards and an inbox. 

In terms of data sharing, my opinion is that the focus of the SCTA team needs to go one step below the ideal database schema.

Our focus should instead be on specifying or adopting a common serialization (or common serializations).

The reason for this is because we not only want the flexibility to ingest this information in different database,
but the ability to manipulate this data into different data models and ontology.

Thus, the ontology we use for serialization does not necessarily have to be the end result that is served via the SCTA or the only ontology we can use.

Case in point: We shouldn't have to struggle with whether or not the SCTA graph should be based on the the Basel Base Knora ontology. We should be able transform our data into both ontologies and then see over time which approach is best serving our needs.

Thus documentation that promotes agreement and common practice is much more important than getting database schema exactly right.

We've already achieved this the LombardPress-Schema.

We need something similar for:

- manuscript-info and descriptions
- prosopographies
- questions lists
- canonical quotation lists

We're already close with these, but we just need to standardize this.

If our preferred serialization is JSON-LD, then we're really just talking about a graph.

If we can agree on rules for how these graphs should be serialized, individuals and groups can publish partial graphs, and these graphs can easily be connected, compared, and reconciled.

# Using an LDN Inbox for aggregation:

An LDN inbox is ideal place for this reconciliation.

Any anonymous system can send a post to this inbox.

The inbox application can immediately validate any posted information and reject anything that does not meet the agreed about specification.

Since the inbox would support understood ontologies (we can support more than one), the inbox could recognize conflict or disagreements in submissions. The inbox could have an admin portal as well, in which SCTA maintainers could be alerted to conflicts or disagreements.

A read-only could be alerted to re-writes of the existing graph, check for changes using a shasum alogrithm, and if there have been changes, re-ingest the updated graph.

The database, in this case would simply be the result of merge these distributed graphs together and indexing them.

# Example

Imagine the following scenario.

UI Applications designed to help users input new data should ideally load their displays after a check of the SCTA database.

If someone in Copenhagen adds a birthdate for Aquinas on Monday, the submission of this information (regardless of what happens locally) should trigger a notification to the inbox for Aquinas. If this is new information from a trusted source (we could support anonymous contributions as well), this information will be automatically appended to the existing graph.

A change to the graph should trigger a partial run of the SCTA database build script (whether automatically or as the result of a daily cron job).

Then on Tuesday, if someone in Basel intends to update the entry for Aquinas, the date of Aquinas will appear, and if there is no disagreement, then there will be no need to update the field. If the editor, disagrees with this new information, they could change the date on the Basel system. And again, whatever happens, locally, a new notification would be triggered. This notification would be recognized by the inbox as a conflict, and flagged for review by the SCTA maintainers. SCTA editors could decide to use one date or another, or allow both to exist side by side in the SCTA database.

# Dealing with different ids.

Different systems may use different ids.

Inboxes can accommodate multiple ids for the same resource by using the owl:sameAs property. In order to make this association, the publisher will need to make an initial discovery about an already existing id for the target resource (perhaps the SCTA id, or the Viaf id).

Once they discover, for example the Viaf id: "Aquinas", then can send to the inbox of that target a notification asserting the owl:sameAs relationship to a new id, for example "ThomasAquinas". Once the association is made a system could send a notification to the same inbox using any of the known ids. There could be many ids connected by an owl:sameAs relationship. It wouldn't matter. The same information would be retrieved when any of the ids are called. Conflict ids, would be flagged by the inbox and reviewed.
