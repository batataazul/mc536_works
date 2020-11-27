# Lab07

## Exercício 1

Calcule o Pagerank do exemplo da Wikipedia em Cypher:

~~~cypher
LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/santanche/lab2learn/master/network/pagerank/pagerank-wikipedia.csv' AS line

MERGE (p1:Page {name:line.source})

MERGE (p2:Page {name:line.target})

CREATE (p1)-[:LINKS]->(p2)



match(n)

return n



CALL gds.graph.create(

  'prGraph2',

  'Page',

  'LINKS'

)



CALL gds.pageRank.stream('prGraph2')

YIELD nodeId, score

MATCH (p:Page {name: gds.util.asNode(nodeId).name})

SET p.pagerank = score


match (p1:Page)-[l:LINKS]->(p2:Page)

return p1.name, p2.name, p1.pagerank


match (p1:Page)

return p1.name, p1.pagerank

~~~

## Exercício 2

Departing from a Drug-Drug graph created in a previous lab, whose relationship determines drugs taken together, apply a community detection in it to see the results:

~~~cypher
CALL gds.graph.create(

  'communityGraphDrug',

  'Drug',

  {

    Relates: {

      orientation: 'UNDIRECTED'

    }

  }

)


CALL gds.louvain.stream('communityGraphDrug')

YIELD nodeId, communityId

RETURN gds.util.asNode(nodeId).name AS name, communityId

ORDER BY communityId ASC


CALL gds.louvain.stream('communityGraphDrug')

YIELD nodeId, communityId

MATCH (p:Drug {code: gds.util.asNode(nodeId).code})

SET p.community = communityId


CALL gds.louvain.stream('communityGraphDrug')

YIELD nodeId, communityId

RETURN gds.util.asNode(nodeId).name AS name, communityId
~~~
