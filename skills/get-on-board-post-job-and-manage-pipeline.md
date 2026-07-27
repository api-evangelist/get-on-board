---
name: Post a job and manage the hiring pipeline
description: Create a job posting, submit it for moderation, then move applicants through hiring-process phases using the private Companies API.
api: openapi/get-on-board-openapi-original.yml
operations: [createCompanyPrivateJob, submitCompanyPrivateJob, publishCompanyPrivateJob, listJobProcesses, listApplications, listProcessPhases, listApplicationDiscardReasons, moveApplicationPhase]
auth: ApiKeyAuth (company API key, Bearer)
---

# Post a job and manage the hiring pipeline

Uses the **private Companies API**. Authenticate with your company API key:
`Authorization: Bearer <api_key>` (find it at
`https://www.getonbrd.com/companies/[your-company]/api_settings`). Requires an
active subscription that includes API access. Test against
`https://sandbox.getonbrd.dev/api/v0/` first.

## Steps

1. **Create the job** — `POST /api/v0/jobs` (`createCompanyPrivateJob`).
2. **Submit for moderation** — `POST /api/v0/jobs/{job_id}/submit`
   (`submitCompanyPrivateJob`). Jobs must be approved before going live.
3. **Publish once approved** — `POST /api/v0/jobs/{job_id}/publish`
   (`publishCompanyPrivateJob`).
4. **Find the hiring process** — `GET /api/v0/jobs/{job_id}/processes`
   (`listJobProcesses`). Use the job `id` slug returned by the jobs list.
5. **List applicants for a process** —
   `GET /api/v0/applications?process_id={process_id}` (`listApplications`).
6. **Discover target phases** — `GET /api/v0/processes/{process_id}/phases`
   (`listProcessPhases`). Phase IDs are numeric.
7. **If the target phase is `kind: "discarded"`**, fetch valid reasons with
   `GET /api/v0/applications/discard_reasons` (`listApplicationDiscardReasons`).
8. **Move the applicant** — `PATCH /api/v0/applications/{application_id}/phase`
   (`moveApplicationPhase`), passing a `discard_reason` id when moving to a
   discarded phase.

## Rules
- No idempotency key exists — do not blindly retry writes; check state first.
- Errors are `{ message, code }`; `401 unauthorized`, `404 not_found` (or expired
  subscription), `422 unprocessable_content` for validation.
- Requests are `application/x-www-form-urlencoded`; responses `application/json`.
