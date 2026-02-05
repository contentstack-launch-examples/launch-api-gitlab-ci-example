# Deploy to Contentstack Launch with GitLab CI using Launch API

This repository shows how to deploy a Next.js app to **Contentstack Launch** using the [Launch Public API](https://www.contentstack.com/docs/developers/apis/launch-api) file upload with **GitLab CI**. The pipeline (`.gitlab-ci.yml`) runs on push to `main` and uses the deployment script `deploy-api.js`.

---

## What you need

The Launch API supports [**M2M**, **OAuth**, or **Authtoken**](https://www.contentstack.com/docs/developers/apis/launch-api#authentication). This repo uses **M2M** (Client ID + Client Secret).

| Variable | Description |
|----------|-------------|
| `CONTENTSTACK_CLIENT_ID` | M2M/OAuth app ID |
| `CONTENTSTACK_CLIENT_SECRET` | M2M/OAuth app secret |
| `CONTENTSTACK_REGION` | Region: <small>`AWS_NA`, `AWS_EU`, `AWS_AU`, `AZURE_NA`, `AZURE_EU`, `GCP_NA`, `GCP_EU`</small> |
| `PROJECT_UID` | Launch project UID |
| `ENVIRONMENT_UID` | Launch environment UID |

## Quick start

1. Clone or copy this repo, then **push it to a GitLab project** (create a new project in GitLab and push this code).
2. Create an app with `launch:manage` or `launch.projects:write` scope (this repo uses M2M; OAuth/Authtoken are also supported by the API).
3. Set the required variables as CI/CD variables (Settings → CI/CD → Variables), or use `.env` for local runs.
4. Push to main (or run `npm run deploy` locally).

**Run locally:** Copy `.env.example` to `.env`, fill in values, then run `npm run deploy`.

## GitLab CI

**Pipeline:** `.gitlab-ci.yml` — on push to main: checkout → `npm install form-data archiver dotenv` → `node deploy-api.js`.

**Variables** (Settings → CI/CD → Variables): `CONTENTSTACK_CLIENT_ID`, `CONTENTSTACK_CLIENT_SECRET`, `CONTENTSTACK_REGION`, `PROJECT_UID`, `ENVIRONMENT_UID`.

---

## What gets included in the deployment zip

The deploy script (`deploy-api.js`) zips only the files and folders listed in the **`essentialFiles`** array inside `createZipFile()`. There is no config file — the list is hardcoded.

**Default list:** `package.json`, `package-lock.json`, `next.config.js`, `pages`, `public`, `app`, `functions`

**To customize:** Edit `deploy-api.js` and change the `essentialFiles` array in `createZipFile()` so it matches your project (e.g. add `src`, `components`, or `lib`; remove `app` or `functions` if you don't use them). Only entries in that array are included in the zip.

---

## References

- [Launch API – Authentication](https://www.contentstack.com/docs/developers/apis/launch-api#authentication)
- [Contentstack OAuth](https://www.contentstack.com/docs/developers/developer-hub/contentstack-oauth)
- [Contentstack Launch API](https://www.contentstack.com/docs/developers/apis/launch-api)
