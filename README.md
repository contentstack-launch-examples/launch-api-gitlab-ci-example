# Redeploy to Contentstack Launch with GitLab CI using Launch API

This repository demonstrates how to redeploy a Next.js application to **Contentstack Launch** using the [Launch Public API](https://www.contentstack.com/docs/developers/apis/launch-api) file upload with **GitLab CI**. After the first deployment is done from the **Launch UI** or **Launch APIs**, the pipeline (`.gitlab-ci.yml`) runs on every push to `main` and redeploys the project using the script `deploy-api.js`.

---

## Prerequisites

The Launch API supports [**M2M**, **OAuth**, or **Authtoken**](https://www.contentstack.com/docs/developers/apis/launch-api#authentication). This example uses **M2M** (Client ID and Client Secret).

| Variable | Description |
|----------|-------------|
| `CONTENTSTACK_CLIENT_ID` | M2M or OAuth application ID |
| `CONTENTSTACK_CLIENT_SECRET` | M2M or OAuth application secret |
| `CONTENTSTACK_REGION` | Region: <small>`AWS_NA`, `AWS_EU`, `AWS_AU`, `AZURE_NA`, `AZURE_EU`, `GCP_NA`, `GCP_EU`</small> |
| `PROJECT_UID` | Launch project UID |
| `ENVIRONMENT_UID` | Launch environment UID |


## Deployment flow

1. **First deployment:** Perform the initial deployment from the **Launch UI** or **Launch APIs**. Create the project and deploy once.
2. **Subsequent deployments:** Every push to `main` redeploys the project automatically through this GitLab CI pipeline.

## Quick start

1. Perform the first deployment from the **Launch UI** or **Launch APIs**.
2. Clone or copy this repository and push the code to a GitLab project.
3. Create an application with `launch:manage` or `launch.projects:write` scope (M2M is used in this example; OAuth and Authtoken are also supported by the API).
4. Add all required variables to GitLab CI/CD Variables or to `.env` for local runs.
5. Push to `main` to trigger a redeploy, or run `npm run deploy` locally.

**Running locally:** Copy `.env.example` to `.env`, enter your values, then run `npm run deploy`.

## GitLab CI

**Pipeline:** The file `.gitlab-ci.yml` runs on push to `main`: it checks out the code, runs `npm install form-data archiver dotenv`, then runs `node deploy-api.js`.

**Required variables** (Settings → CI/CD → Variables): `CONTENTSTACK_CLIENT_ID`, `CONTENTSTACK_CLIENT_SECRET`, `CONTENTSTACK_REGION`, `PROJECT_UID`, `ENVIRONMENT_UID`.

---

## What is included in the deployment zip

The deploy script (`deploy-api.js`) includes in the zip only the files and folders listed in the **`essentialFiles`** array inside `createZipFile()`. The list is defined in code; there is no separate configuration file.

**Default entries:** `package.json`, `package-lock.json`, `next.config.js`, `pages`, `public`, `app`, `functions`

**To customize:** Edit `deploy-api.js` and update the `essentialFiles` array in `createZipFile()` to match your project (for example, add `src`, `components`, or `lib`; remove `app` or `functions` if not used). Only entries in that array are included in the zip.

---

## References

- [Launch API – Authentication](https://www.contentstack.com/docs/developers/apis/launch-api#authentication)
- [Contentstack OAuth](https://www.contentstack.com/docs/developers/developer-hub/contentstack-oauth)
- [Contentstack Launch API](https://www.contentstack.com/docs/developers/apis/launch-api)
