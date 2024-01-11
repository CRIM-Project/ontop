 Repository
It contains files for the SPARQL Endpoint based on OnTop (https://ontop-vkg.org/) relative to the CRIM project. 

The Endpoint is available at: https://lod.crimproject.org/


# TEST Query

PREFIX foaf: <http://xmlns.com/foaf/0.1/>
SELECT ?name ?mbox
WHERE { ?x foaf:name ?name .
        OPTIONAL { ?x foaf:mbox ?mbox }
      }
