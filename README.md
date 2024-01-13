# Repository
It contains files for the SPARQL Endpoint based on OnTop (https://ontop-vkg.org/) relative to the CRIM project. 

The Endpoint is available at: https://lod.crimproject.org/

# Examples of SPARQL queries

Copy-paste the queries in the SPARQL Endpoint available at the link above

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
Select masses and their sections with title and fulltitle.

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

## Query 4)
Select music expressions that are not masses, their title, fulltitle, and composer.

```sparql
SELECT ?composition ?title ?fulltitle ?vgenre ?composerName WHERE
{?composition a omac:MusicExpression;
dcterms:title ?title;
dbp:fullTitle ?fulltitle;
dbp:genre/omac:hasValue ?vgenre;
dbp:composer/schema:name ?composerName;

FILTER(?vgenre!= data:genre-mass)
} order by ?vgenre
```

## Query 5)
Select structure observations, the analytic segment, attribute, and attribute value they concern. 
Retrieve also the fulltitle of the composition to which the segment is related.

```sparql
SELECT ?strObs ?segment ?composition ?title
?schemaValue WHERE
{?strObs a omac:StructureObservation;
omac:concernsSegment ?segment;
omac:concernsAttribute ?schemaAttr;
omac:concernsAttributeValue ?schemaValue.
  
?segment a omac:AnalyticSegment;
omac:segmentOf ?composition.
?composition dbp:fullTitle ?title}
```

## Query 6)
Select similarity observations with the models and derivatives they concern, and the similarity type they ascribe.

```sparql
SELECT ?simObs ?similarity ?model ?derivative 
WHERE
{?simObs a omac:SimilarityObservation;
omac:concernsModel ?model;
omac:concernsDerivative ?derivative;
omac:concernsSimilarity/rdfs:label ?similarity.
  
?model a omac:AnalyticSegment.
?derivative a omac:AnalyticSegment}
```

## Query 7)
Select music expressions standing in a model-derivative relation.

```sparql
SELECT ?obsSim ?model ?modelTitle ?derivative ?derivativeTitle
WHERE
{?obsSim a omac:SimilarityObservation;
omac:concernsModel ?modelSeg;
omac:concernsDerivative ?derivSeg.
  
?derivSeg omac:segmentOf ?derivative.    
?modelSeg omac:segmentOf ?model.
?model omac:modelFor ?derivative.

 ?model dbp:fullTitle ?modelTitle.
?derivative dbp:fullTitle ?derivativeTitle}
```







