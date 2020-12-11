# Lab08

## Tarefas com Publicações

## Questão 1
Construa uma comando SELECT que retorne dados equivalentes a este XPath
~~~xquery
//individuo[idade>20]/@nome
~~~

### Resolução
~~~sql
select I.nome
from individuo I
where I.idade > 20
~~~

## Questão 2
Qual a outra maneira de escrever esta query sem o where?

~~~xquery
let $fichariodoc := doc('https://raw.githubusercontent.com/santanche/lab2learn/master/xml/fichario.xml')
 
for $i in ($fichariodoc//individuo)
where $i[idade>17]
return {data($i/@nome)}
~~~
### Resolução
~~~xquery
let $fichariodoc := doc('https://raw.githubusercontent.com/santanche/lab2learn/master/xml/fichario.xml')

return $fichariodoc//individuo[idade>17]/@nome
~~~

## Questão 3
Escreva uma consulta SQL equivalente ao XQuery:
~~~xquery
let $fichariodoc := doc('https://raw.githubusercontent.com/santanche/lab2learn/master/xml/fichario.xml')

for $i in ($fichariodoc//individuo)
where $i[idade>17]
return {data($i/@nome)}
~~~

### Resolução
~~~sql
select I.nome
from individuo I
where I.idade > 17
~~~

## Questão 4
Retorne quantas publicações são posteriores ao ano de 2011.

### Resolução
~~~xquery
let $pub := doc('https://raw.githubusercontent.com/santanche/lab2learn/master/data/publications/publications.xml')
for $i in ($pub//publication[year > 2011])
let $maiorOnze := $i/ano
group by $maiorOnze
return {count($i)}
~~~

## Questão 5
Retorne a categoria cujo `<label>` em inglês seja 'e-Science Domain'.

### Resolução
~~~xquery
let $pub := doc('https://raw.githubusercontent.com/santanche/lab2learn/master/data/publications/publications.xml')
for $c in ($pub//categories[@catkey='subject']/category)
return {$c[label = 'e-Science Domain']}
~~~

## Questão 6
Retorne as publicações associadas à categoria cujo `<label>` em inglês seja 'e-Science Domain'. A associação entre o label e a key da categoria deve ser feita na consulta.

### Resolução
~~~xquery
let $pub := doc('https://raw.githubusercontent.com/santanche/lab2learn/master/data/publications/publications.xml')
for $c in ($pub//categories[@catkey='subject']/category),
    $i in ($pub//publication)
where $i/key = $c/@key and $c/label[@lang = 'en-US'] = 'e-Science Domain'
return data($i/title)

~~~

## Tarefas com DRON e PubChem

## Questão 1

Liste o nome de todas as classificações que estão apenas dois níveis imediatamente abaixo da raiz.

### Resolução
~~~xquery
let $dron := doc('https://raw.githubusercontent.com/santanche/lab2learn/master/data/faers-2017-dron/dron.xml')

for $i in $dron/*//*//*
return (data($i/@name),'&#xa;')
~~~

## Questão 2

Apresente todas as classificações de um componente a sua escolha (diferente de `Acetylsalicylic Acid`) que estejam hierarquicamente dois níveis acima. Note que no exemplo dado em sala foi considerado um nível hierárquico acima.

### Resolução
~~~xquery
(escreva aqui a resolução em XQuery)
~~~

## Questão 3

### Questão 3.1

Liste todos os códigos ChEBI dos componentes disponíveis.

#### Resolução
~~~xquery
let $chebi := doc('https://raw.githubusercontent.com/santanche/lab2learn/master/data/pubchem/pubchem-chebi-synonyms.xml')

for $i in ($chebi/PC-DataSet/InformationList/Information),
    $ped in ($i//Synonym)
return if (substring(data($ped),1,5) = 'CHEBI')
then (data($ped),'&#xa;')
else ('')

~~~

### Questão 3.2

Liste todos os códigos ChEBI e primeiro nome (sinônimo) de cada um dos componentes disponíveis.

#### Resolução
~~~xquery
(escreva aqui a resolução em XQuery)
~~~

### Questão 3.3

Para cada código ChEBI, liste os sinônimos e o nome que aparece para o mesmo componente no DRON (se existir). Para isso faça um JOIN com o DRON.

#### Resolução
~~~xquery
(escreva aqui a resolução em XQuery)
~~~