# Lau Trials (Embark) Shopify theme: required workflow

## Context
You are working on the Lau Trials Shopify store (lau-trials.myshopify.com), a test store for the Embark Fine Jewelry theme.
The theme code lives in GitHub: https://github.com/laura58clicks/Embark
The `PDP` branch is the base branch for the theme. It is connected to the Shopify theme "Embark/PDP". Anything merged into `PDP` is synced to that theme.
Each team member has their own unpublished PREVIEW theme in Shopify for checking changes.

## Hard rules
1. NEVER edit the live (published) theme directly. Do not use the Shopify theme editor or code editor on the live theme, and never push files to the live/published theme with the Shopify CLI. Never use the `--live`, `--allow-live` or `--publish` flags.
2. All changes go through GitHub: a new branch, then a pull request, then a merge into `PDP`.
3. NEVER commit or push directly to `PDP`.
4. Do not force-push, rewrite history, or delete branches you did not create.
5. Never commit secrets (API keys, tokens, passwords, .env files).

## Step-by-step
1. Get the latest code:
   `git fetch origin && git checkout PDP && git pull origin PDP`
2. Create a branch from `PDP` with a descriptive name:
   - `fix/<short-description>` for bug fixes
   - `feat/<short-description>` for new features or sections
   - `perf/<short-description>` for speed improvements
   - `cleanup/<short-description>` for removals

   Example: `git checkout -b fix/product-page-variant-picker`
3. Make the smallest change that does the job. Do not reformat or touch unrelated files.
4. Before a large removal or cleanup, make a separate first commit that backs up the files you are about to delete or change (commit message starting with "Backup:").
5. Write clear commit messages that say what changed and why, e.g. "Header: remove Blog link from desktop and mobile menus".
6. Preview the change on the user's own preview theme (never the live theme):
   - Run `shopify theme list --store lau-trials.myshopify.com` and find the user's preview theme (an unpublished theme named like "Preview - <their name>"). Ask the user which one is theirs if it is unclear.
   - If they don't have one yet, create it: `shopify theme push --unpublished --store lau-trials.myshopify.com` and name it "Preview - <their name>".
   - Push the branch to it: `shopify theme push --theme <preview theme ID> --store lau-trials.myshopify.com`
   - Before pushing, double-check the theme ID is NOT the live theme (role: live / main) and NOT the GitHub-connected "Embark/PDP" theme.
   - Give the user the preview link and ask them to check the change.
7. Push the branch to GitHub and open a pull request into `PDP`. In the description include:
   - What changed and why
   - Which files changed
   - The preview link and which pages to check
   - Any risk or anything to double-check
8. Merge only after the user has looked at the preview and confirmed it is good:
   - Pull the latest `PDP` again and merge it into your branch first (see conflicts below).
   - Merge the pull request into `PDP`, then tell the user the change is now in the "Embark/PDP" theme.
   - Afterwards, check the affected pages on that theme's preview.

## Watch out for theme editor conflicts
Edits made in the Shopify theme editor on the "Embark/PDP" theme are synced back into GitHub, mostly in JSON files:
- `config/settings_data.json`
- `sections/*.json` (e.g. `sections/header-group.json` holds the header menu)
- `templates/*.json`

If your change touches any of these:
- Pull the latest `PDP` right before merging, and merge it into your branch.
- Change only the specific keys you need. Never replace the whole file with an older copy, because that would erase other people's theme editor changes.
- Mention in the PR that JSON settings files were changed.

## If something is unclear
Ask the user instead of guessing, especially before deleting anything, before changes that affect checkout, pricing, subscriptions or tracking, or if the request seems to need a live-theme edit.
