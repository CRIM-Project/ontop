# Repository
It contains files for the SPARQL Endpoint based on OnTop (https://ontop-vkg.org/) relative to the CRIM project. 

The Endpoint is available at: https://lod.crimproject.org/

# Examples of SPARQL queries

You can copy-paste these queries in the SPARQL Endpoint available at the link above

## List of namespaces used in the queries:

```sparql
prefix xsd: <http://www.w3.org/2001/XMLSchema#>
Prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#>
prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
prefix dcterms: <http://purl.org/dc/terms/>
prefix dbp: <http://dbpedia.org/property/>
prefix dbo: <http://dbpedia.org/ontology/>
prefix schema: <https://schema.org/>
prefix omac: <https://omac.crimproject.org/>
prefix data: <https://data.crimproject.org/> 
```


## Query 1)
Select persons and their names. Retrieve their VIAF ids and roles if available. 

```sparql
SELECT ?person ?person_name ?role ?viaf WHERE
 {?person a schema:Person;
  schema:name ?person_name.
optional
{?person omac:hasRole ?role}
optional
{?person <http://www.wikidata.org/prop/direct/P214> ?viaf}
} order by ?person
```

## Query 2)
Select persons who are composers of masses. For masses, retrieve their titles.  

```sparql
SELECT ?composer ?composerName ?composition  ?compositionTitle
{?composer a schema:Person;
  	schema:name ?composerName;
  	omac:composerOf ?composition.
  
?composition a omac:MusicExpression;
   dcterms:title ?compositionTitle;
    dbp:genre/omac:hasValue data:genre-mass.
} order by ?composer
```

## Query 3)
```sparql
SELECT ?composition ?compositionTitle ?section ?sectionTitle ?sectionFullTitle
{
?composition a omac:MusicExpression;
dcterms:title ?compositionTitle;
dbp:genre/omac:hasValue data:genre-mass;
omac:hasSection ?section.

?section dcterms:title ?sectionTitle;
  dbp:fullTitle ?sectionFullTitle

} order by ?composition
```




