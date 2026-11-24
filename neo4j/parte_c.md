\# Parte C — Grafos no Neo4j (RA 242507172)



\## C1. Modelagem do Domínio e Subgrafo (`EMPRESA\_SANCIONADA: E12`)



```mermaid

graph LR

&#x20;   E1\[":Empresa<br/>{cnpj, nome, sancionada}"] -->|":SOCIA\_DE {pct}"| E2\[":Empresa<br/>{cnpj, nome, sancionada}"]

&#x20;   C1\[":Container<br/>{id, classe}"] -->|":PERTENCE\_A"| E1

&#x20;   C1 -->|":PASSOU\_POR {data}"| T\[":Terminal<br/>{nome}"]

&#x20;   C1 -->|":EMPILHADO\_SOBRE"| C2\[":Container<br/>{id, classe}"]

```



\### Subgrafo de `E12`

```cypher

MATCH (s:Empresa {cnpj: 'E12'})<-\[r:SOCIA\_DE]-(socio:Empresa) RETURN socio, r, s

```

\* \*\*Resultado obtido:\*\* `\[COLE AQUI OS SÓCIOS DE E12]`



\---



\## C2. Investigação Societária e Operacional (`E12` até 3 saltos)



```cypher

MATCH (s:Empresa {cnpj: 'E12'})-\[:SOCIA\_DE\*1..3]-(inv:Empresa)

MATCH (s)<-\[:PERTENCE\_A]-(cs:Container)-\[ps:PASSOU\_POR]->(t:Terminal)<-\[pi:PASSOU\_POR]-(ci:Container)-\[:PERTENCE\_A]->(inv)

WHERE ps.data = pi.data AND inv <> s

RETURN DISTINCT inv.nome AS empresa\_investigada, ci.id AS container, t.nome AS terminal, pi.data AS data

```

\* \*\*Resultado obtido:\*\* `\[COLE AQUI A TABELA RETORNADA]`



\---



\## C3. Debug Cypher



1\. \*\*(i) Duplicação de nós com `CREATE`:\*\* Buscar os nós existentes com `MATCH` antes de criar a relação:

&#x20;  ```cypher

&#x20;  MATCH (a:Empresa {cnpj:'E1'}), (b:Empresa {cnpj:'E2'}) CREATE (a)-\[:SOCIA\_DE {pct:30}]->(b)

&#x20;  ```

2\. \*\*(ii) Explosão combinatória em ciclos:\*\* Definir direção e limite de saltos (`\*1..5`) com `DISTINCT`:

&#x20;  ```cypher

&#x20;  MATCH (s:Empresa {sancionada:true})-\[:SOCIA\_DE\*1..5]->(x:Empresa) RETURN DISTINCT x.nome

&#x20;  ```

3\. \*\*(iii) Direção invertida na pilha (`K48`):\*\* Inverter a seta para buscar os contêineres acima da base:

&#x20;  ```cypher

&#x20;  MATCH (base:Container {id:'K48'})<-\[:EMPILHADO\_SOBRE\*1..3]-(x:Container) RETURN x.id

&#x20;  ```

&#x20;  \* \*\*Contêineres retornados:\*\* `\[COLE OS IDs RETORNADOS]`

4\. \*\*(iv) Contagem duplicada de passagens:\*\* Usar `count(DISTINCT k)` para evitar contar o mesmo contêiner múltiplas vezes:

&#x20;  ```cypher

&#x20;  MATCH (k:Container)-\[:PASSOU\_POR]->(t:Terminal) WHERE k.classe = 'Perigoso' RETURN t.nome, count(DISTINCT k)

&#x20;  ```



