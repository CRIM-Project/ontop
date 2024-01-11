# Repository
It contains files for the SPARQL Endpoint based on OnTop (https://ontop-vkg.org/) relative to the CRIM project. 

The Endpoint is available at: https://lod.crimproject.org/

```
SELECT ?person ?person_name ?role ?viaf
 {?person a schema:Person;
  schema:name ?person_name.
optional
{?person omac:hasRole ?role}
optional
{?person <http://www.wikidata.org/prop/direct/P214> ?viaf}
} order by ?person

```
