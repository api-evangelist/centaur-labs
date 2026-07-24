---
name: download-labeling-results
description: Find a Centaur task and retrieve aggregated labeling results, including a bulk results archive.
api: Centaur Public API
server: https://api.centaurlabs.com
operations:
  - task_list_tasks_public_v1_list_get
  - get_results-public-v1-get
  - get_results-public-v2-list
  - get_results-public-v2-getresultsarchiveurl
  - post_reads-public-v1-lookup
---

# Download labeling results

Retrieve aggregated results (and raw opinions) from a completed Centaur task. All
requests go to `https://api.centaurlabs.com` with your `X-API-KEY` header.

## Steps

1. **Locate the task** — `GET /tasks/public/v1/list`
   (`task_list_tasks_public_v1_list_get`), optionally filtering by project,
   organization, or `search_term`. Capture the task ID.

2. **Fetch results**:
   - Single case: `GET /results/public/v1/get` or `/results/public/v2/get`
     (`get_results-public-v1-get`).
   - Page of cases: `GET /results/public/v2/list` (`get_results-public-v2-list`)
     — iterate pages until exhausted.

3. **Bulk export** — `GET /results/public/v2/getResultsArchiveUrl`
   (`get_results-public-v2-getresultsarchiveurl`) returns a signed URL to an
   archive of results files; download it directly.

4. **Inspect individual opinions (optional)** — `POST /reads/public/v1/lookup`
   (`post_reads-public-v1-lookup`) to retrieve the individual reads (annotator
   opinions) that were aggregated into a result.

## Conventions

- Prefer the v2 results endpoints; v1 remains for single-case lookups.
- List endpoints are paginated — follow pages rather than assuming one response.
- See `data-model/centaur-labs-data-model.yml` for Case → Read → Result.
