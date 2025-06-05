# 🚀 Deploy Hugo Site to S3

This repository contains a GitHub Actions workflow that builds and deploys a [Hugo](https://gohugo.io/) static site to an Amazon S3 bucket. The deployment target and subdomain are selected manually when running the workflow.

## 📦 Features

- Uses Hugo `v0.101.0`
- Deploys to either `qa` or `prod` S3 buckets
- Supports multiple subdomains like `products`, `docs`, `about`, etc.
- Dynamically selects Hugo config file and base URL
- Invalidates CloudFront cache
- Manual dispatch with input selection

## 🛠️ Requirements

### Repository Secrets

Before running the workflow, make sure the following [secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets) are configured:

| Secret Name            | Description                              |
|------------------------|------------------------------------------|
| `READ_PAT`             | Personal access token with `repo` scope  |
| `AWS_ACCESS_KEY_ID`    | AWS access key for deployment            |
| `AWS_SECRET_ACCESS_KEY`| AWS secret key for deployment            |

> 🔐 These secrets are used for checking out the private repo and authenticating with AWS.

---

## 🚀 Usage

1. Go to the **Actions** tab in the repository.
2. Select **Deploy Site** workflow.
3. Click **Run workflow**.
4. Choose the desired `target` and `subdomain`.

### 🧠 Target & Subdomain Logic

| Target | Subdomain | Base URL                        | Config File                    | S3 Bucket                |
|--------|-----------|----------------------------------|--------------------------------|--------------------------|
| `prod` | `home`    | https://www.groupdocs.net/       | `./configs/www.groupdocs.net.toml` | `www.groupdocs.net`     |
| `prod` | `products`| https://products.groupdocs.net/  | `./configs/products.groupdocs.net.toml` | `www.groupdocs.net`     |
| `qa`   | `home`    | https://qa.groupdocs.net/        | `./configs/www.groupdocs.net.toml` | `qa.groupdocs.net`      |
| `qa`   | `products`| https://products-qa.groupdocs.net/| `./configs/products.groupdocs.net.toml` | `qa.groupdocs.net` |

---

## 🧩 Workflow Overview

The workflow does the following:

1. **Checks out** the `groupdocs-net/groupdocs-net` repo.
2. **Sets up** Hugo with extended features.
3. **Determines** the base URL and config file based on selected inputs.
4. **Builds** the site using `hugo --minify`.
5. **Deploys** to the appropriate S3 bucket and invalidates CloudFront CDN.

---

## 📂 Folder Structure

Your Hugo config files should be placed in the `configs/` directory and named using this pattern:

```log
configs/
├── www.groupdocs.net.toml
├── products.groupdocs.net.toml
├── docs.groupdocs.net.toml
...
```
