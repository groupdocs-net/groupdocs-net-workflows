# Hugo Site Deployment

This repository contains a GitHub Actions workflow to manually deploy a [Hugo](https://gohugo.io/) static site to AWS S3 and CloudFront.

## 🚀 Deployment Workflow

The `Deploy Home` workflow builds the Hugo site using a specified base URL and deploys it to the appropriate S3 bucket using `hugo deploy`.

### 🧭 How to Use

1. Go to the **Actions** tab in this repository.
2. Select the **Deploy Home** workflow.
3. Click **Run workflow**.
4. Choose the target environment:
   - `qa` — Deploys to `https://qa.groupdocs.net/`
   - `prod` — Deploys to `https://www.groupdocs.net/`

### ⚙️ Configuration

- **Hugo version**: `0.101.0`
- **Build config file**: `./configs/www.groupdocs.net.toml`
- **Deployment targets** are defined in `config.toml` under:
  - `qa` → QA
  - `prod` → PROD

--

### 📂 Workflow File

See `.github/workflows/deploy_home.yml` for the complete workflow configuration.
