# kilicasa-recruiting/.github

Special org-profile repository for the **kilicasa-recruiting** GitHub organisation.

## What this repo contains

| Path | Purpose |
|------|---------|
| `profile/README.md` | Org profile page — visible at https://github.com/kilicasa-recruiting |
| `.github/ISSUE_TEMPLATE/apply.yml` | GitHub Issue Form — the public self-service application entry point |
| `.github/workflows/onboard-candidate.yml` | Automation triggered on every `apply`-labelled issue |
| `.github/CODEOWNERS` | Restricts workflow/template modifications to `@jordanvalnet` |

## How the self-service onboarding works

1. A candidate visits https://github.com/kilicasa-recruiting and clicks **Apply / Postuler**.
2. GitHub opens the Issue Form (requires GitHub login — captures the candidate's username automatically).
3. On issue submission, `onboard-candidate.yml` fires:
   - Posts an immediate "setting up…" acknowledgement comment (~5 s).
   - Creates a private repo `kilicasa-recruiting/<github_username>` from the `recruiting-template`.
   - Disables forking on the new repo.
   - Invites the candidate as a collaborator with `push` permission.
   - Writes `candidates/<github_username>.json` to `kilicasa-recruiting/recruiting-admin` via the GitHub Contents API (no clone required).
   - Posts a final comment with the repo link + next steps (FR + EN).
   - Closes the issue.
4. **Idempotent**: re-submitting the form redirects the candidate to their existing repo without creating a duplicate.

## Required secret — GH_ADMIN_TOKEN

The workflow uses `secrets.GH_ADMIN_TOKEN`. This is the **same fine-grained PAT** Jordan already created for the recruiting org. It must be set as a **repository secret** on this repo:

> **Repo Settings → Secrets and variables → Actions → New repository secret**
> Name: `GH_ADMIN_TOKEN`
> Value: the PAT value

The PAT needs the following permissions on `kilicasa-recruiting/*`:
- `Administration: write` (create repos)
- `Contents: write` (write candidate JSON to `recruiting-admin`)
- `Issues: write` (post comments, close issues)
- `Metadata: read` (implied)

## End-to-end test (Jordan)

After setting `GH_ADMIN_TOKEN`:

1. Go to https://github.com/kilicasa-recruiting
2. Click **Apply / Postuler** — you should land on the issue form.
3. Check both confirmation boxes and click **Submit new issue**.
4. Watch the issue — within ~30 s you should see the first bot comment ("Création en cours…").
5. Within ~60 s you should see the second bot comment with:
   - A link to `https://github.com/kilicasa-recruiting/jordanvalnet` (your own username)
   - The "Prochaines étapes / Next steps" instructions
6. The issue should be automatically closed.
7. Verify:
   - `https://github.com/kilicasa-recruiting/jordanvalnet` exists and is **private**.
   - You are listed as a collaborator with `push` access.
   - `candidates/jordanvalnet.json` exists in `kilicasa-recruiting/recruiting-admin`.
8. Clean up after the test: delete `kilicasa-recruiting/jordanvalnet` and remove `candidates/jordanvalnet.json` from `recruiting-admin`.

## Manual steps after push

- [ ] **Set `GH_ADMIN_TOKEN`** secret in this repo's settings (see above).
- [ ] **Enable the org profile page**: in the `kilicasa-recruiting` org settings, the profile README is usually picked up automatically once this repo is public and `profile/README.md` exists. If it doesn't appear, go to *Org Settings → Profile* and confirm the README is enabled.
