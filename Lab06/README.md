# Tarefa de Cypher e o FDA Adverse Event Reporting System (FAERS)

## Exercício 1

Escreva uma sentença em Cypher que crie o medicamento de nome `Metamizole`, código no DrugBank `DB04817`.

### Resolução
~~~cypher
CREATE (:Drug {drugbank: "DB04817", name:"Metamizole"}) 
~~~

## Exercício 2

Considerando que a `Dipyrone` e `Metamizole` são o mesmo medicamento com nomes diferentes, crie uma aresta com o rótulo `:SameAs` que ligue os dois.

### Resolução
~~~cypher
MATCH (d1:Drug {name:"Dipyrone"}) 

MATCH (d2:Drug {name:"Metamizole"}) 

CREATE (d1)-[:SameAs]->(d2) 
~~~

## Exercício 3

Use o `DELETE` para excluir o relacionamento que você criou (apenas ele).

### Resolução
~~~cypher
match(d1:Drug {name:"Dipyrone"}) 

match(d2:Drug {name: "Metamizole"}) 

match(d1) -[e:SameAs]->(d2) 

delete e 
~~~

## Exercício 4

Faça a projeção em relação a Patologia, ou seja, conecte patologias que são tratadas pela mesma droga.

### Resolução
~~~cypher
match(p1:Pathology)<-[r:sameTreat]->(p2:Pathology) 

where r.weight > 20 

return p1,p2 

limit 20 
~~~

## Exercício 5

Construa um grafo ligando os medicamentos aos efeitos colaterais (com pesos associados) a partir dos registros das pessoas, ou seja, se uma pessoa usa um medicamento e ela teve um efeito colateral, o medicamento deve ser ligado ao efeito colateral.

### Resolução
~~~cypher
LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/santanche/lab2learn/master/data/faers-2017/drug-use.csv' AS line 

CREATE (:Person { Id: line.idperson, Drug: line.codedrug}) 


CREATE INDEX ON :Person(Id) 

LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/santanche/lab2learn/master/data/faers-2017/sideeffect.csv' AS line2 

match(P1:Pathology) 

match(P2:Person) 

match(D1:Drug) 

where P1.code = line2.codePathology and P2.Id = line2.idPerson and D1.code = P2.Drug 

merge (D1)-[s:SideEffect]->(P1) 

ON CREATE SET s.weight=1 

ON MATCH SET s.weight=s.weight+1 

 
~~~

## Exercício 6

Que tipo de análise interessante pode ser feita com esse grafo?

Proponha um tipo de análise e escreva uma sentença em Cypher que realize a análise.

### Resolução

Uma análise possível é verificar os pesos das arestas para descobrir quais são os remédios que apresentam mais vezes apresentaram mais vezes os mesmos efeitos colaterais e quais são esses efeitos colaterais associados a esses remédios.
~~~cypher
MATCH p=(D1:Drug)-[r:SideEffect]->(P1:Pathology)  

where r.weight > 50 

RETURN D1 

LIMIT 50 
 

MATCH p=(D1:Drug)-[r:SideEffect]->(P1:Pathology)  

where r.weight > 50 

RETURN P1 

LIMIT 50 

 
MATCH p=(D1:Drug)-[r:SideEffect]->(P1:Pathology)  

where r.weight > 50 

RETURN P1, D1, r 

LIMIT 50 
~~~
