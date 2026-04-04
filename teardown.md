# Teardown — Database Administration Project

A two-phase project covering backup, restore, user management, indexing, and automation across PostgreSQL (Phase 1) and MySQL (Phase 2), running on WSL (Windows Subsystem for Linux).

---

## Stack Choices & Rationale

| Component | Decision Rationale |
|---|---|
| **PostgreSQL (Phase 1)** | The dominant open-source relational database in the modern data stack. `pg_dump` / `pg_restore` are the standard tools for logical backup and restore. Role-based access control (RBAC) is a critical production skill. |
| **MySQL (Phase 2)** | Widely used in web application stacks. Covers different backup semantics (`mysqldump`), storage engine considerations (InnoDB vs MyISAM), and indexing strategies. |
| **Bash automation (`mybackup.sh`)** | Demonstrates that database operations can and should be scripted — reducing human error in routine maintenance and enabling scheduling via cron. |
| **WSL (Windows Subsystem for Linux)** | Enables Linux-native database tooling on a Windows development machine — a realistic constraint for many learners and enterprise developers. |

---

## Key Design Decisions

- **Both phases include a restore step**, not just backup. Backup without verified restore is meaningless — this is a critical operational principle.
- **User management and role privileges** are covered in Phase 1 because access control is a prerequisite to safe backup/restore operations in a multi-user environment.
- **MySQL Phase 2 includes a storage engine check** — understanding whether a table uses InnoDB (transactional, crash-safe) or MyISAM (non-transactional) affects backup strategy.
- **Bash automation (`mybackup.sh`)** makes the backup routine repeatable and schedulable, bridging DBA work with systems engineering.

---

## Trade-offs

| Decision | Benefit | Cost |
|---|---|---|
| `pg_dump` (logical backup) | Human-readable SQL output, portable across versions | Slower than physical backup (`pg_basebackup`) for large databases; cannot do point-in-time recovery without WAL archiving |
| `mysqldump` over Percona XtraBackup | No extra tools required, widely documented | Full copy every run; XtraBackup supports incremental hot backups for large InnoDB databases |
| WSL over native Linux / Docker | Accessible to Windows users | WSL has I/O overhead and some networking quirks; production work should be on native Linux or containerised |

---

## Extensions & Real-World Use Cases

- Extend to **point-in-time recovery (PITR)** in PostgreSQL using WAL archiving (`archive_mode = on`) and `pg_basebackup` — the production-grade backup strategy for OLTP databases.
- Add **connection pooling (PgBouncer)** to the PostgreSQL setup to demonstrate a production-ready deployment pattern.
- Implement **logical replication** between two PostgreSQL instances to demonstrate high-availability fundamentals.
- Port `mybackup.sh` to an **Airflow DAG** to demonstrate how operational database tasks integrate with a modern orchestration layer.

---

## Portfolio Signal

Most data engineering portfolios skip database administration entirely. Including backup/restore and user management demonstrates understanding of the operational layer that underpins every data platform — not just the transformation and modelling layers on top of it.
