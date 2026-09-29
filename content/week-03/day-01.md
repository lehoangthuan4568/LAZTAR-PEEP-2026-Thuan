+++
title = "Day 01 - 28/09/2026 (Remote)"
weight = 1
+++

# PRACTICE REPORT — DAY 01

## Domain D – Inventory Model: Design Completion & Integration Review

### 1. Key Accomplishments & Implementation Details

* **Design finalized**: Completed and finalized the ERD, Data Dictionary, and Business Rules (11 rules, BR-D-01 → BR-D-11) for Domain D — Inventory Model, covering `qty_on_hand`/`qty_reserved`/`qty_available` logic, optimistic locking via `version`, and unique constraints per `(location_id, sku_id, lot_no/serial_no)`.
* **Cross-domain sync**: Reviewed and re-aligned the Inventory design against Domain B (Warehouse Structure — BIN hierarchy, purpose, `is_pickable`) and Domain C (Product & Supplier — base UOM, lot/serial tracking) after their ERDs were published; updated PK/FK types to `BIGINT` to match the team's actual implementation.
* **Integration review**: Reviewed the merged system-wide ERD (`erd_all`) and cross-checked deliverables from Domain A and Domain E; identified two outstanding open items for team follow-up:
  1. Location handling for in-transit stock during warehouse transfers.
  2. Segregation of duties (proposer ≠ approver) for inventory adjustments — not yet reflected in the final submitted design.
* **Repo & environment setup**: Received repo access, cloned the source code, configured `.env`, and set up a local PostgreSQL instance via Docker per team guidelines — confirmed no direct changes are made to the shared development database.
* **Week 2 API/DTO review**: Reviewed the Week 2 API & DTO specification for Domain D/E and flagged a critical implementation gap — the `/api/inventory/adjust` endpoint currently commits changes directly with no approval workflow — for correction before backend implementation begins.

### 2. Next Steps

* Implement the Prisma schema for `inventory`/`stock_ledger` and build `StockLedgerService` (single-transaction writes, optimistic locking enforcement, append-only guarantee), coordinating with Domain A/B/C/E owners on remaining FK and permission dependencies.
