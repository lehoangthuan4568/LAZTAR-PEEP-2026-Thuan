+++
title = "Day 02 - 30/09/2026 (Office)"
weight = 2
+++

# PRACTICE REPORT — DAY 02

## Domain D & E – Core Inventory Ledger, Triggers, Optimistic Locking & Query APIs

### 1. Key Accomplishments & Implementation Details

* **Database Engine Setup & Synchronization (PostgreSQL & Prisma Migration)**:
  - Initialized all 5 domains from `schema.prisma` into the local PostgreSQL instance via migration `20260930021017_init_all_domains`.
  - Implemented database-level constraints not natively expressible in Prisma schema: 3 CHECK constraints (`chk_qty_on_hand_nonneg`, `chk_qty_reserved_nonneg`, `chk_qty_reserved_le_onhand`), stored generated column `qty_available = qty_on_hand - qty_reserved` (`GENERATED ALWAYS AS ... STORED`), and partial unique index `user_roles_null_wh_uk` for system-wide roles when `warehouse_id IS NULL`.
  - Added migration `20260930022500_enforce_stock_ledger_append_only`: Configured row-level trigger `trg_prevent_stock_ledger_mutation` and statement-level trigger `trg_prevent_stock_ledger_truncate` on `stock_ledger`, strictly blocking `UPDATE`, `DELETE`, and `TRUNCATE` at the database engine level.
  - Seeded standard reference data: 7 WMS roles (`ADMIN`, `MANAGER`, `SUPERVISOR`, `RECEIVER`, `PICKER`, `INSPECTOR`, `VIEWER`), default system admin, 10 movement types with signs (+1/-1), and 6 reference types.

* **Developed `InventoryLedgerService` (Core Inventory Movement Engine)**:
  - **`recordMovement(input)`**:
    - Executes within a single `prisma.$transaction`.
    - Validates strictly positive magnitude `quantity > 0` (`Prisma.Decimal`), rejecting negative and zero quantities with `BadRequestException`.
    - Resolves `movement_types` by code, dynamically calculates `qtyChange = sign * quantity` (never accepts signed quantities directly from callers).
    - Enforces warehouse integrity by validating `location.warehouseId === input.warehouseId`.
    - Automatically initializes a new `inventory` record with `version = 0` upon first appearance of a SKU at a given location.
    - Applies **Optimistic Locking** (`WHERE id = :id AND version = :old_version`, incrementing `version = version + 1`). Throws `ConflictException` (409) on version mismatch for caller retry.
    - Inserts immutable append-only ledger entries into `stock_ledger` storing `qtyBefore`, `qtyChange`, `qtyAfter`, reference document, and audit user.
  - **`reverseMovement(originalLedgerId, actorUserId, note?)`**:
    - Resolves original ledger entry, prevents reversing a record that is already a reversal.
    - Inverts `qtyChange = -original.qtyChange`, updates inventory via optimistic lock, and creates a new reversal record pointing to `reversalOfId = originalLedgerId` (original row remains untouched).
    - **Exception Segregation**: Implemented distinct `AlreadyReversedException` (HTTP 422 Unprocessable Entity - terminal business rule violation, non-retryable) separated from `ConflictException` (HTTP 409 - transient version race condition, retryable) to prevent infinite client retries.
    - **Database-Level Concurrent Reversal Protection**: Added migration `20260930030716_add_unique_reversal_of_id` with partial unique index `stock_ledger_reversal_of_id_uk` on `(reversal_of_id) WHERE reversal_of_id IS NOT NULL`, catching Prisma error `P2002` to rethrow `AlreadyReversedException`.

* **Implemented 2 Inventory Query APIs**:
  - **`GET /api/inventory`**: Supports combined AND filtering by `warehouseId`, `locationId`, `skuId`, `lotNo`. Returns 9 fields (`skuCode`, `locationCode`, `lotNo`, `serialNo`, `qtyOnHand`, `qtyReserved`, `qtyAvailable`, `locationPurpose`, `isPickable`). Preserves raw `locationPurpose` (QC, STORAGE, DAMAGED) and `isPickable` so frontend can aggregate without mixing up QC stock, and inspectors can audit QC items.
  - **`GET /api/stock-ledger`**: Supports filtering by `skuId`, `locationId`, `warehouseId`. Default sorting: `createdAt DESC` (latest first). Returns full movement and audit details.
  - **Safe Pagination**: Handled edge cases gracefully — zero results (`items: []`, `totalItem: 0`, `totalPage: 0`) and out-of-bounds offset (page 999 returning `items: []`) without crashing or throwing undefined errors.

* **Comprehensive Testing & Verification**:
  - Built Jest unit test suite (`inventory-ledger.service.spec.ts`) covering all service methods and pagination edge cases — 15/15 tests passing (`npm run test`). Fixed greeting expectation in `app.controller.spec.ts`.
  - Developed standalone database integration test runner `scripts/run-test-scenarios.ts` equipped with a **Safety Guard** verifying `DATABASE_URL` targets `localhost` / `127.0.0.1` before execution.
  - Executed 9 end-to-end scenarios directly on the PostgreSQL container: negative/zero quantity, invalid movement type, optimistic locking race condition, warehouse mismatch, initial inventory creation, reversal, and **concurrent parallel reversals (`Promise.allSettled`)** — verifying exactly 1 succeeds and 1 is rejected by `AlreadyReversedException`, leaving exactly 1 reversal in the DB.
  - Successfully built production bundle via `npm run build` (0 drift, 0 compilation errors).

* **Git Workflow & Pull Request Submission**:
  - Adhered to strict Git safety guidelines: no direct commits or force-pushes to `PEEP2`.
  - Created feature branch `feature/inventory-ledger-service`, structured into 3 atomic Conventional Commits (`fix(test)`, `feat(inventory)` service, `feat(inventory)` APIs).
  - Pushed feature branch and created **Pull Request #3** targeting base `PEEP2` with full verification report and permission follow-up notes for Domain A.

### 2. Next Steps

* Monitor team code review and coordinate merging Pull Request #3 into `PEEP2`.
* Coordinate with Domain A (Nam Nguyen) to attach RBAC middleware (`ProtectGuard` / permissions) to the query APIs once ready (replacing temporary `@Public()` decorator).
* Prepare implementation for subsequent transactional workflows (Inbound Receipt, Putaway, Pick/Ship, Allocation/Reservation).
