## Competency Questions

FRBR DL can be used for answering several questions related to documents and their different description levels.
In the following subsections, some of them are introduced together with their respective SPARQL queries. 

The prefixes that are used in all the SPARQL queries provided below are defined as follows:

    PREFIX frbr: <http://purl.org/vocab/frbr/core#>
    PREFIX dcterms: <http://purl.org/dc/terms/>

### CQ1

Which expressions realize a given work, along with the creators of that work?

    SELECT ?work ?title ?creator ?expression
    WHERE {
        ?work a frbr:Work ;
            frbr:realization ?expression .
        OPTIONAL { ?work dcterms:title ?title . }
        OPTIONAL { ?work frbr:creator ?creator . }
    }

### CQ2

What are the manifestations embodying an expression, along with their format and producer?

    SELECT ?expression ?manifestation ?producer ?format
    WHERE {
        ?expression a frbr:Expression ;
            frbr:embodiment ?manifestation .
        ?manifestation a frbr:Manifestation .
        OPTIONAL { ?manifestation frbr:producer ?producer . }
        OPTIONAL { ?manifestation dcterms:format ?format . }
    }

### CQ3

What is the hierarchical containment chain of an expression (e.g., article to issue, volume, and journal)?

    SELECT ?childExpression ?parentExpression ?identifier ?description ?title
    WHERE {
        ?childExpression a frbr:Expression ;
            frbr:partOf ?parentExpression .
        OPTIONAL { ?parentExpression dcterms:identifier ?identifier . }
        OPTIONAL { ?parentExpression dcterms:description ?description . }
        OPTIONAL { ?parentExpression dcterms:title ?title . }
    }

### CQ4

What are the full WEMI relationships (Work, Expression, Manifestation) for a publication and its responsible corporate body or persons?

    SELECT ?work ?expression ?manifestation ?producer ?issueDate
    WHERE {
        ?work a frbr:Work ;
            frbr:realization ?expression .
        ?expression a frbr:Expression ;
            frbr:embodiment ?manifestation .
        ?manifestation a frbr:Manifestation .
        OPTIONAL { ?manifestation frbr:producer ?producer . }
        OPTIONAL { ?manifestation dcterms:issued ?issueDate . }
    }