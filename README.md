\# Operação Maré Alta — Auditoria de Bancos NoSQL (MongoDB \& Neo4j)



\* \*\*Aluno:\*\* Arthur Felipe Camargo Costa Vitoria

\* \*\*RA:\*\* 242507172

\* \*\*Disciplina:\*\* Bancos de Dados NoSQL — Avaliação Prática (2º Bimestre)



\---



\## Parâmetros da Prova (Gerados pelo RA `242507172`)



\### 1. MongoDB (`logiporto\_gerador.js`)

\* \*\*CLASSE\_ALVO:\*\* `Refrigerado`

\* \*\*TERMINAL\_ALVO:\*\* `T2`

\* \*\*DATA\_CORTE:\*\* `2026-04-01`

\* \*\*INCIDENTE (Google Maps `lat, lon`):\*\* `-25.4780, -48.4960`

\* \*\*INCIDENTE (GeoJSON `\[lon, lat]`):\*\* `\[-48.4960, -25.4780]`

\* \*\*CÓDIGO DE AUTENTICIDADE:\*\* `LP-242507172-20047-10114`



\### 2. Neo4j (`grafo\_gerador.js`)

\* \*\*EMPRESA\_SANCIONADA:\*\* `E12`

\* \*\*BASE\_DA\_PILHA:\*\* `K48`



\---



\## Ambiente Utilizado

\* \*\*MongoDB:\*\* Contêiner Docker `mongo-lab` (`mongosh`, banco `logiporto` com 400.000 movimentações e 100.000 alertas).

\* \*\*Neo4j:\*\* Contêiner Docker `neo4j-lab` (`neo4j:5`, Neo4j Browser via `http://localhost:7474`).

\* \*\*Controle de Versão:\*\* Git / GitHub com pré-registro cronológico de hipóteses antes da execução das consultas.



\---



\## Estrutura do Repositório



\* `README.md` — Identificação do aluno, ambiente, parâmetros determinísticos do RA e Código de Autenticidade.

\* `previsoes.md` — Pré-registro de hipóteses técnicas (estágios do plano, ordem de grandeza e comportamento esperado) commitado antes da execução das queries.

\* `diario.md` — Log de bordo cronológico registrando comandos executados, erros diagnosticados e validações ao longo das etapas.

\* `mongo/parte\_a.md` — Consultas, saídas de `.explain("executionStats")`, tabela comparativa ESR e análises das questões \*\*A1, A2, A3 e A4\*\*.

\* `mongo/parte\_b.md` — Pipelines de agregação, análise de desempenho, debug arquitetural e auditoria final de índices das questões \*\*B1 e B2\*\*.

\* `neo4j/grafo.cypher` — Script Cypher determinístico gerado a partir do RA `242507172` para popular o grafo.

\* `neo4j/parte\_c.md` — Diagrama do modelo de domínio (Mermaid), subgrafo da empresa sancionada `E12`, investigação societária/operacional e correções Cypher das questões \*\*C1, C2 e C3\*\*.

s

