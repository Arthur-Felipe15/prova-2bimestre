* A1-b: IXSCAN + FETCH + SORT em memoria, \~12000 docs examinados, nReturned 20
* A1-c: IXSCAN + FETCH + LIMIT sem SORT em memoria, dezenas de docs examinados, nReturned 20
* A2: Erro sem indice; apos 2dsphere usa GEO\_NEAR\_2DSPHERE, dezenas de docs examinados, ate 10 retornados
* A3: TTL falha porque registrado\_em foi gravado como String ISO e nao BSON Date
* A4: Com \_id:0 usa IXSCAN + PROJECTION\_COVERED (totalDocsExamined 0); sem \_id:0 adiciona FETCH
* B1: IXSCAN cobrindo classe e ts antes do group, \~40000 docs agrupados, retorna 3 terminais
* B2: Original retorna kg\_total 0 por falta de $ e project precoce; corrigido usa IXSCAN e retorna 3 terminais
* C2. Investigação societária e operacional (`EMPRESA\_SANCIONADA: E12`)

  * Direção esperada das arestas:
* &#x20;	 No modelo gerado, a relação `(a)-\[:SOCIA\_DE {pct}]->(b)` indica que `a` detém participação societária em `b`.
* &#x20; 	Para investigar empresas ligadas à sancionada `E12` em até 3 níveis sem gerar caminhos redundantes nem entrar em ciclos infinitos, utiliza-se travessia direcionada com profundidade limitada (`\*1..3`) a partir de `E12` (`(s:Empresa {cnpj: 'E12'})-\[:SOCIA\_DE\*1..3]->(inv:Empresa)` ou no sentido inverso `<-` para mapear controladores), cruzando com `(Container)-\[:PERTENCE\_A]->(Empresa)` e `(Container)-\[:PASSOU\_POR]->(Terminal)` com `DISTINCT`.
* &#x20; 	Ordem de grandeza de registros retornados:
* &#x20;	 Baixa, na ordem de \*\*unidades a poucas dezenas de registros\*\* (aproximadamente entre `0` e `25` linhas únicas).
* &#x20;  	Justificativa: Como o grafo possui 30 empresas (`NE = 30`), 90 contêineres (`NC = 90`, média de 3 por empresa) e apenas 2 passagens por contêiner distribuídas entre 5 terminais e 30 dias de outubro de 2026 (`150` combinações possíveis de terminal + data), a coincidência simultânea de mesmo terminal e mesma data exata entre contêineres de `E12` e de empresas conectadas a ela em até 3 saltos é altamente seletiva.



&#x20;C3. Comportamento esperado no Debug Cypher (Previsão)

&#x20; 	(i) `CREATE` direto sem `MATCH`: Criará dois nós novos e duplicados com `cnpj: 'E1'` e `cnpj: 'E2'` sem propriedades `nome` ou `sancionada`, em vez de conectar os nós já existentes no grafo.

&#x09;(ii) `\[:SOCIA\_DE\*]` sem limite e sem direção:\*\* Em um grafo denso com 45 ligações societárias entre 30 empresas (contendo ciclos), a ausência de limite de saltos (`\*1..N`) e de direção (`-`) causará explosão combinatória de caminhos e repetição massiva dos mesmos nomes.

&#x09;(iii) `(base:Container {id:'K48'})-\[:EMPILHADO\_SOBRE\*1..3]->(x)`: Retornará 0 registros, pois `K48` é a `BASE\_DA\_PILHA` (`48 % 4 = 0`) e o relacionamento `EMPILHADO\_SOBRE` aponta de cima para baixo (do topo para a base), exigindo a inversão da seta (`<-\[:EMPILHADO\_SOBRE\*1..3]-`).

&#x09;(iv) `count(k)` sem `DISTINCT`: Superestimará o total de contêineres perigosos por terminal, pois contêineres que passaram duas vezes pelo mesmo terminal serão contados em duplicidade.

