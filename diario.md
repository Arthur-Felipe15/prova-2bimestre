
## 3. Parte B — Aggregation Pipeline no MongoDB (Finalização)

* **B1 (Relatório Gerencial — Classe Refrigerado a partir de 2026-04-01):**
  * **Indexação:** Criei o índice composto { classe: 1, ts: 1, terminal: 1, peso_kg: 1 } para otimizar o estágio inicial $match e permitir leitura eficiente antes do agrupamento.
  * **Execução e Plano:** Estruturei o pipeline iniciando com $match (classe: "Refrigerado" e ts >= ISODate("2026-04-01")), seguido de $group por $terminal (total de movimentações, peso total, peso médio e maior peso), $sort decrescente por peso_total, $limit: 3 e $project renomeando _id para terminal.
  * **Observação no .explain("executionStats"):** Confirmei a filtragem antecipada via índice antes do $group, reduzindo o volume processado em relação aos 400.000 documentos totais.

* **B2 (Debug Arquitetural e Auditoria de Índices):**
  * **Diagnóstico das 4 Falhas:** (1) $project inicial descartava peso_kg (erro de lógica); (2) $sum: "peso_kg" sem cifrão somava string literal resultando em 0 (erro de lógica); (3) $limit: 3 antes de $sort cortava grupos antes de ordenar (erro de lógica); (4) $match no final impedia uso de índice e processava toda a coleção (erro de desempenho).
  * **Correção e Auditoria:** Reposicionei $match no início, corrigi "$peso_kg" e coloquei $limit: 3 após $sort. Ao final, rodei db.movimentacoes.getIndexes() confirmando apenas os índices necessários ativos.
