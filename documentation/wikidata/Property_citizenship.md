## Get the most frequent citizenships

* If your population is in great number, use the [QLever SPARQL Endpoint](https://qlever.dev/wikidata)
* In the case of this population, we observe that the main occupations are the same that the ones used to define the population. The information appears to be less relevant to answer research questions.

```
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd:  <http://www.w3.org/2001/XMLSchema#>
PREFIX wd:   <http://www.wikidata.org/entity/>
PREFIX wdt:  <http://www.wikidata.org/prop/direct/>

SELECT ?citizenship ?citizenshipLabel (COUNT(DISTINCT ?item) AS ?eff)
WHERE {
  {
    SELECT DISTINCT ?item
    WHERE {
      ?item wdt:P31 wd:Q5 ;
            wdt:P569 ?birthDate .

      FILTER("1801-00-00T00:00:00Z"^^xsd:dateTime <= ?birthDate &&
             ?birthDate < "1992-00-00T00:00:00Z"^^xsd:dateTime)

      { ?item wdt:P106 wd:Q2306091 }   # occupation: sociologist
      UNION
      { ?item wdt:P101 wd:Q21201 }     # field of work: sociology
    }
  }

  ?item wdt:P27 ?citizenship .
  ?citizenship rdfs:label ?citizenshipLabel .
  FILTER(LANG(?citizenshipLabel) = "en")
}
GROUP BY ?citizenship ?citizenshipLabel
ORDER BY DESC(?eff)
LIMIT 30
```

### Most frequent occupations

| object                                  | objectLabel        | eff   |
| --------------------------------------- | ------------------ | ----- |
| http://www.wikidata.org/entity/Q169470  | physicist          | 26091 |
| http://www.wikidata.org/entity/Q1622272 | university teacher | 8094  |
| http://www.wikidata.org/entity/Q11063   | astronomer         | 6804  |
| http://www.wikidata.org/entity/Q170790  | mathematician      | 2299  |
| http://www.wikidata.org/entity/Q1650915 | researcher         | 1403  |
| http://www.wikidata.org/entity/Q752129  | astrophysicist     | 1064  |
| http://www.wikidata.org/entity/Q81096   | engineer           | 1029  |
| http://www.wikidata.org/entity/Q593644  | chemist            | 951   |
| http://www.wikidata.org/entity/Q901     | scientist          | 869   |
| http://www.wikidata.org/entity/Q36180   | writer             | 868   |
