# Redeploy to Contentstack Launch with GitLab CI using Launch API

This repository shows how to **redeploy** a Next.js app to **Contentstack Launch** using the [Launch Public API](https://www.contentstack.com/docs/developers/apis/launch-api) file upload with **GitLab CI**. After the first deployment is done from the Launch UI, the pipeline (`.gitlab-ci.yml`) runs on every push to `main` and redeploys the project via the script `deploy-api.js`.

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


## Deployment flow

1. **First deployment:** Do it from the **Launch UI**. Create the project and deploy once.
2. **Later:** Every push to `main` redeploys the project automatically via this GitLab CI pipeline.

## Quick start

1. Deploy once from the **Launch UI**.
2. Clone or copy this repo and push it to a GitLab project.
3. Create an app with `launch:manage` or `launch.projects:write` scope (M2M; OAuth/Authtoken also supported).
4. Add all required variables to GitLab CI/CD Variables or to `.env` for local runs.
5. Push to `main` to redeploy (or run `npm run deploy` locally).

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
