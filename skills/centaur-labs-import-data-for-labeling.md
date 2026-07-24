---
name: import-data-for-labeling
description: Create a Centaur project, import data from cloud storage or URLs, and create a labeling task with answer classes.
api: Centaur Public API
server: https://api.centaurlabs.com
operations:
  - create_project_public_projects_public_v1_add_post
  - add_import_imports_public_v1_add_post
  - add_assets_from_urls_imports_public_v1_addassetsfromurls_post
  - get_import_public_imports_public_v1_get_get
  - task_add_public_tasks_public_v1_add_post
  - handler_add_answer_labelclasses_public_v1_add_post
---

# Import data for labeling

Set up a Centaur labeling project end to end. All requests go to
`https://api.centaurlabs.com` and must include your `X-API-KEY` header (generate
the key + password in Portal > Organization Settings > API Integration; see
`authentication/centaur-labs-authentication.yml`).

## Steps

1. **Create a project** — `POST /projects/public/v1/add`
   (`create_project_public_projects_public_v1_add_post`). Supply the organization
   and project name/data type. Keep the returned project ID.

2. **Import your data** into the project:
   - From cloud storage: `POST /imports/public/v1/add`
     (`add_import_imports_public_v1_add_post`) with the source URI and target
     task/project. Use `GET /imports/public/v1/validateAccess` and
     `GET /imports/public/v1/policy` first if the bucket needs an access policy.
   - From a list of HTTPS URLs: `POST /imports/public/v1/addAssetsFromUrls`
     (`add_assets_from_urls_imports_public_v1_addassetsfromurls_post`).

3. **Poll import status** — `GET /imports/public/v1/get`
   (`get_import_public_imports_public_v1_get_get`) until the import reports its
   assets/cases created.

4. **Create the labeling task** — `POST /tasks/public/v1/add`
   (`task_add_public_tasks_public_v1_add_post`) with the project ID, task type,
   and prompt. Use `/tasks/public/v2/add` if you need label class groups or
   free-text questions.

5. **Add answer classes** — `POST /labelClasses/public/v1/add`
   (`handler_add_answer_labelclasses_public_v1_add_post`) to attach the answer
   choices annotators will select. Do NOT use the deprecated
   `/answers/public/v1/add`.

## Conventions

- Paths are versioned per app (`/{app}/public/v{N}/...`); prefer v2 where offered.
- No Idempotency-Key is supported; re-check import status rather than blind-retry.
- See `conventions/centaur-labs-conventions.yml` for pagination and error shape.
