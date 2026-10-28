# Hourly QA leadership job board

A static page (`docs/jobs.html`) that shows jobs scored against your resume. A GitHub Action runs every
hour, checks company career pages (Greenhouse, Lever, Ashby, optional Adzuna), scores new postings with
Claude, and updates `docs/jobs.json`. The page reads that file, so it is always current.

## Setup (about 20 minutes)
1. Create a GitHub repo and upload all files in this folder, including `.github/workflows/hourly.yml`.
   (If your portfolio already lives in a repo with a `docs` folder, copy `docs/jobs.html` and
   `docs/jobs.json` into it, and the other files to the repo root.)
2. Add these in Settings > Secrets and variables > Actions > New repository secret:
   - `ANTHROPIC_API_KEY`: from console.anthropic.com
   - `RESUME_TEXT`: paste the full text of your resume (this keeps it out of the public repo)
   - optional `ADZUNA_APP_ID` and `ADZUNA_APP_KEY`: free at developer.adzuna.com
3. Settings > Pages > Build and deployment: Source "Deploy from a branch", branch `main`, folder `/docs`.
4. Actions tab > "hourly-job-update" > **Run workflow**. After a minute or two, open
   `https://YOUR-USERNAME.github.io/YOUR-REPO/jobs.html`.
5. Add companies in `companies.json`. The name is the last part of the careers URL:
   `job-boards.greenhouse.io/qualia/...` -> `qualia` under `greenhouse`;
   `jobs.lever.co/jobgether` -> `jobgether` under `lever`; `jobs.ashbyhq.com/ashby` -> `ashby` under `ashby`.
   Optional: add a nicer display name under `names`. Wrong names are skipped.

## Notes
- **The site is public.** Anyone with the link sees the jobs and match scores. Do not link it from
  your resume if you would rather keep your search private.
- GitHub free Pages requires a public repo. Do not put your resume in the repo (use the secret).
- "Every hour" is approximate; GitHub can start scheduled runs several minutes late.
- A job is scored once. Jobs drop off the page about 2 days after they disappear from the source.
- Scores are estimates. The match slider on the page defaults to 60.
- If the repo is quiet for ~60 days GitHub may pause the schedule; commits from the bot usually prevent this,
  but if updates stop, re-enable the workflow in the Actions tab.
