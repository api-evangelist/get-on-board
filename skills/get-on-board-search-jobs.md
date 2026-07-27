---
name: Search tech jobs on Get on Board
description: Browse categories and search the public Get on Board job board for Latin America — no authentication required.
api: openapi/get-on-board-openapi-original.yml
operations: [listCategories, searchJobs, listCategoryJobs, retrieveCompany]
auth: none (public API)
---

# Search tech jobs on Get on Board

Use the **public** Get on Board API to discover published tech jobs. No token required.
Base URL: `https://www.getonbrd.com/api/v0/` (sandbox: `https://sandbox.getonbrd.dev/api/v0/`).

## Steps

1. **List categories** — `GET /api/v0/categories` (`listCategories`). Each category has an
   `id` slug (e.g. `programming`, `machine-learning-ai`) you can filter jobs by.
2. **Search jobs** — `GET /api/v0/search/jobs` (`searchJobs`). At least one of
   `query`, `companies`, `featured`, `remote`, `country_code`, or `board_host` is
   **required** — otherwise you get `422 unprocessable_content`. Example:
   `GET /api/v0/search/jobs?query=rails&remote=true`.
3. **Or list a category's jobs** — `GET /api/v0/categories/{category_id}/jobs`
   (`listCategoryJobs`).
4. **Inspect a company** — `GET /api/v0/companies/{id}` (`retrieveCompany`).

## Conventions
- Paginate with `page` / `per_page` (default 120, max 120); read `meta.total_pages`.
- Expand relationships with `expand[]` (e.g. `expand[]=tags`), nested via dot notation.
- Localize with `?lang=en|es|pt`.
- Errors come back as `{ message, code }` (not RFC 9457).
