
- A1-b: IXSCAN + FETCH + SORT em memoria, ~12000 docs examinados, nReturned 20

- A1-c: IXSCAN + FETCH + LIMIT sem SORT em memoria, dezenas de docs examinados, nReturned 20

- A2: Erro sem indice; apos 2dsphere usa GEO_NEAR_2DSPHERE, dezenas de docs examinados, ate 10 retornados

- A3: TTL falha porque registrado_em foi gravado como String ISO e nao BSON Date

- A4: Com _id:0 usa IXSCAN + PROJECTION_COVERED (totalDocsExamined 0); sem _id:0 adiciona FETCH

- B1: IXSCAN cobrindo classe e ts antes do group, ~40000 docs agrupados, retorna 3 terminais
