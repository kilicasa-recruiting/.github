# kilicasa-recruiting/.github

Special org-profile repository for the **kilicasa-recruiting** GitHub organisation.

## What this repo contains

| Path | Purpose |
|------|---------|
| `profile/README.md` | Org profile page — visible at https://github.com/kilicasa-recruiting |
| `.github/ISSUE_TEMPLATE/apply.yml` | Public apply issue form with prefilled machine-detectable fields |
| `.github/ISSUE_TEMPLATE/config.yml` | Keeps blank issues disabled so candidates use the dedicated apply form |
| `.github/workflows/onboard-candidate.yml` | Admin-triggered onboarding automation for private candidate intake |
| `.github/CODEOWNERS` | Restricts workflow/template modifications to `@jordanvalnet` |

## Candidate apply link

- Apply form: https://github.com/kilicasa-recruiting/.github/issues/new?template=apply.yml

## Manual private intake fallback

The public apply issue form is the default candidate path. This workflow-dispatch flow remains available as an admin fallback.

Fallback path:

1. The candidate shares their GitHub username with a recruiter.
2. A recruiter/admin triggers `onboard-candidate.yml` manually from the Actions tab with:
   - `candidate`: GitHub username
   - `email`: optional candidate email captured off-GitHub
3. The workflow:
   - Creates the private repo `kilicasa-recruiting/<github_username>` from the `recruiting-template` if needed.
   - Re-applies the collaborator invite with `push` access so reruns can recover access problems.
   - Ensures the `submitted` label exists on the candidate repo.
   - Creates or updates `candidates/<github_username>.json` in `kilicasa-recruiting/recruiting-admin`.
   - Writes an execution summary in the workflow run instead of posting public issue comments.

The workflow is safe to re-run for the same username. It serializes runs per candidate and will not recreate an existing repository.

## Required secret — GH_ADMIN_TOKEN

The workflow uses `secrets.GH_ADMIN_TOKEN`. This is the **same fine-grained PAT** Jordan already created for the recruiting org. It must be set as a **repository secret** on this repo:

> **Repo Settings → Secrets and variables → Actions → New repository secret**
> Name: `GH_ADMIN_TOKEN`
> Value: the PAT value

The PAT needs the following permissions on `kilicasa-recruiting/*`:
- `Administration: write` (create repos)
- `Contents: write` (write candidate JSON to `recruiting-admin`)
- `Metadata: read` (implied)

## End-to-end test (Jordan)

After setting `GH_ADMIN_TOKEN`:

1. Open **Actions → onboard-candidate → Run workflow**.
2. Fill:
   - `candidate`: `jordanvalnet`
   - `email`: your recruiter-side email value (optional)
3. Start the workflow.
4. Verify the workflow summary reports either repo creation or repo reuse.
5. Verify:
   - `https://github.com/kilicasa-recruiting/jordanvalnet` exists and is **private**.
   - You are listed as a collaborator with `push` access.
   - `candidates/jordanvalnet.json` exists in `kilicasa-recruiting/recruiting-admin`.
6. Clean up after the test: delete `kilicasa-recruiting/jordanvalnet` and remove `candidates/jordanvalnet.json` from `recruiting-admin`.

## Manual steps after push

- [ ] **Set `GH_ADMIN_TOKEN`** secret in this repo's settings (see above).
- [ ] **Train recruiters on the fallback private intake workflow**: mark complete once the recruiting team confirms the process.
- [ ] **Enable the org profile page**: in the `kilicasa-recruiting` org settings, the profile README is usually picked up automatically once this repo is public and `profile/README.md` exists. If it doesn't appear, go to *Org Settings → Profile* and confirm the README is enabled.
