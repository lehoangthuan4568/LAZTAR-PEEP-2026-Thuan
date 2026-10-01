+++
title = "Day 03 - 01/10/2026 (Remote)"
weight = 3
+++

# PRACTICE REPORT — DAY 03

## Domain D & E – Security Hardening, Stock Ledger Reversal API, Pickable Inventory Summary & Conflict Resolution

### 1. Key Accomplishments & Implementation Details

* **Database Engine Security Hardening (Block TRUNCATE on `stock_ledger`)**:
  - Authored and applied Prisma migration `20261001062000_block_truncate_stock_ledger` on the local PostgreSQL container.
  - Added a table-level statement trigger `trg_prevent_stock_ledger_truncate` on `stock_ledger` invoking `prevent_stock_ledger_mutation()`.
  - Together with the existing row-level trigger, this guarantees that `stock_ledger` is 100% immutable and strictly append-only at the database engine level: completely blocking `UPDATE`, `DELETE` (DML) and `TRUNCATE` (DDL), even from direct administrative connections or ORMs.

* **Hardened Inventory Read APIs (ProtectGuard Integration)**:
  - Removed the temporary `@Public()` decorator from read endpoints in `InventoryController` (`GET /api/inventory`) and `StockLedgerController` (`GET /api/stock-ledger`).
  - Restored both APIs under the global `ProtectGuard` mechanism (configured in `main.ts` and integrated with Domain A's Auth/RBAC module).
  - Verified security posture: Requests now strictly require a valid JWT Bearer token in the `Authorization` header, immediately rejecting unauthenticated access with `401 Unauthorized`.
  - Packaged the security fixes into branch `PEEP2-feature/inventory-security-hardening` and submitted **Pull Request #8** targeting `PEEP2`. The PR was reviewed and merged into the main branch by tech lead `MinhTrungPham`.

* **Implemented Stock Ledger Reversal API (`POST /api/stock-ledger/:id/reverse`)**:
  - Exposed `InventoryLedgerService.reverseMovement()` as a REST API endpoint.
  - **Parameters & DTO**: Accepts route param `:id` (original ledger entry ID) and optional body `ReverseStockLedgerBodyDto` (`{ note?: string }`). Automatically retrieves `actorUserId` from the decoded token (`req.user.id`).
  - **Role-Based Authorization**: Restricts the reversal operation strictly to managerial roles: `ADMIN`, `WH_MANAGER` (official role code standardized with Domain A), and `SUPERVISOR`. Non-managerial roles (`VIEWER`, `PICKER`, etc.) are rejected with `403 Forbidden`. Included technical `TODO` comment for seamless transition to generic permissions when Domain A finishes core permission tagging.
  - **Standard RESTful Error Handling**: Accurately maps domain exceptions to HTTP status codes: `404 Not Found` (entry missing), `400 Bad Request` (attempting to reverse a reversal entry), and `422 Unprocessable Entity` (`AlreadyReversedException` on duplicate reversal attempts).
  - Returns the newly created reversal ledger record with consistent fields (`movementTypeCode: "REVERSAL"`, `reversalOfId`, inverted `qtyChange`).

* **Implemented Pickable Inventory Summary API (`GET /api/inventory/summary`)**:
  - Solved a core business risk: Prevents frontend clients from naively summing all inventory rows, which would incorrectly treat items in Quality Control (QC) or DAMAGED locations as available for sale/dispatch.
  - **Pickable-Location Filtering**: Calculates sums (`qtyOnHand`, `qtyReserved`, `qtyAvailable`) exclusively from inventory rows where `location.isPickable = true` (storage and picking locations).
  - **Transparency on Excluded Stock**: Returns `excludedLocationsCount` (number of inventory rows excluded due to `isPickable = false`), allowing operators and frontend interfaces to see that inventory exists in QC/repair without triggering false alarms of lost stock.
  - **Safe & Robust Validation**: Enforces mandatory `skuId` (throwing `400 Bad Request` if omitted). Supports optional `warehouseId` scoping. Safely returns zeros when an SKU has no records or only non-pickable inventory, preventing server errors or crashes.

* **Comprehensive Testing & Multi-Branch Conflict Resolution**:
  - **Unit Testing**: Added 4 unit tests for `getInventorySummary` in `inventory-ledger.service.spec.ts`, bringing total Jest test coverage to **19/19 tests passing** (`npm run test`).
  - **Live Integration Testing**: Verified 8 end-to-end scenarios against the local PostgreSQL container (manager vs viewer roles, parallel double reversal rejection, pickable vs QC aggregation math, 401 unauthenticated check).
  - **Git Conflict Resolution**: After PR #8 was merged into `PEEP2`, branch PR #9 developed minor conflicts in `inventory.controller.ts` and `stock-ledger.controller.ts`. Performed clean `git merge origin/PEEP2`, preserving all new features while removing `@Public()` decorators, and audited both files to ensure zero unused imports or dead decorators.
  - **Cross-Team Clarification & PR #9 Reopen**: Addressed accidental PR closure by a member of the parallel team (`PEEP1`) due to multi-team shared repository naming confusion. Re-synced latest commits from `origin/PEEP2` (incorporating FE PR #10 and SKU UOM conversion PR #11), passed compilation cleanly (`npm run build` exit code 0), and successfully reopened PR #9 with `CLEAN` and `MERGEABLE` status.

### 2. Next Steps

* Monitor team review and coordinate with tech lead (`MinhTrungPham`) to merge Pull Request #9 into `PEEP2`.
* Coordinate with the Frontend team (who recently landed initial layouts in PR #10) to wire up detailed inventory views, pickable stock summary widgets, and role-guarded ledger reversal actions.
* Prepare design and implementation for upcoming Mini-WMS workflow modules: Advanced Location Hierarchy/Capacity, Inbound Putaway, and Outbound Order Allocation.
