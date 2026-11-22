# Major concept-level gaps (whole topics absent from new notes)
1. GROUP BY, HAVING, and aggregate functions (COUNT, SUM, AVG, MIN, MAX). Old Group.md covers these in depth. The new TRUNK skips aggregation entirely, which is a glaring hole — aggregation is half of what people use SQL for.
2. VIEWS and Materialized Views. Stub in Old, listed in the Postgres checklist, totally absent from new. Worth a LEAF — especially the materialised vs regular distinction.
3. CTEs (the WITH clause). Empty stub in Old, completely absent from new. CTEs are core to readable complex SQL and recursive queries.
4. CASE WHEN ... THEN ... ELSE ... END expressions and COALESCE. Listed in old Functions and Expressions.md. Not in new.
5. SQL vs NoSQL comparison. Old has a solid comparison including the four NoSQL types (document / key-value / column-family / graph). The new TRUNK is purely about relational and never positions it against alternatives.
6. OLTP vs OLAP (transactional vs analytical), and HTAP hybrids. Old has a thorough note. New has nothing on workload classification, star/snowflake schemas, or denormalisation for analytics.
7. Persistence design approaches (pure in-memory, in-memory with snapshots, disk-based update-in-place, commit-log-based). Old Persistance.md. The new note covers durability but doesn't situate it against the alternatives.
8. CROSS JOIN and NATURAL JOIN, plus the ON vs USING distinction. Old Joins.md lists them as TODO flashcards but at least flags them. New only covers INNER/LEFT/RIGHT/FULL OUTER.
9. LIMIT / OFFSET / pagination. Empty Old stub, missing from new.
10. Sequences / auto-increment. Empty stub in Old, missing from new.
11. Foreign key referential actions (ON DELETE CASCADE, SET NULL, RESTRICT). New only mentions FK as a constraint; the action behaviour is the more interesting bit and matters in practice.
12. CHECK constraint in detail (mentioned once in passing in Consistency, no dedicated treatment).
13. Migration scripts as a concept (schema evolution without data loss). Old has it, new doesn't.
14. Scheduled jobs (OS cron vs DB-native schedulers like pg_cron, SQL Server Agent, MySQL Event Scheduler).
15. Search-optimised databases (Elasticsearch / OpenSearch / Solr, inverted indexes, R-trees). Old has a brief note; new doesn't mention this database category at all.

# Depth gaps within topics the new notes do cover
1. Index types beyond B-Tree: Hash, GiST, GIN, SP-GiST, BRIN. New only discusses B-Tree. GIN especially matters for JSONB and full-text search.
2. Multi-column indexes, partial indexes, unique indexes, expression indexes — none of these variations covered.
3. Transaction states / lifecycle (Active → Partially Committed → Committed/Failed → Terminated). Old Transactions.md covers this; new skips the state machine.
4. Locks and deadlock detection. Old Postgres checklist flags it. New touches isolation but never gets to lock granularity, lock types (row/table/advisory), or deadlock resolution.
5. Trigger detail: BEFORE vs AFTER timing, FOR EACH ROW vs statement-level, the categories (DDL / DML / Logon). New TRUNK has the audit-trail example but skips the mechanics.
6. Higher normal forms (BCNF, 4NF, 5NF). New acknowledges they exist but doesn't explain BCNF, which is the only one above 3NF that comes up regularly.
7. Denormalisation as a deliberate strategy (when/why to undo normalisation, especially for OLAP). New normalisation note treats it as a one-way street.

