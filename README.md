# Metaplay GitHub Workflows

GitHub Actions for building, testing, and deploying Metaplay game servers.

For a complete guide on setting up your CI pipeline, see [Setup CI Pipeline](https://docs.metaplay.io/cloud-deployments/setup-ci-pipeline).

## Actions

### `setup-cli` — Install the Metaplay CLI

Installs the Metaplay CLI on Linux, macOS, and Windows runners. Optionally authenticates to Metaplay cloud using machine credentials.

#### Deploy to Metaplay cloud

```yaml
- name: Setup Metaplay CLI
  uses: metaplay-shared/github-workflows/setup-cli@v0
  with:
    credentials: ${{ env.METAPLAY_CICD_BOT_CREDENTIALS }}

- name: Generate unique image tag (SHA alone does not guarantee uniqueness)
  run: echo "IMAGE_TAG=$(date -u +%Y%m%d-%H%M%S)-$GITHUB_SHA" >> $GITHUB_ENV

- name: Build image
  run: metaplay build image gameserver:${{ env.IMAGE_TAG }}

- name: Deploy server
  run: metaplay deploy server my-environment gameserver:${{ env.IMAGE_TAG }}
```

#### Run integration tests without cloud access

Set `login: false` to skip the machine login step when cloud access isn't needed.

```yaml
- name: Setup Metaplay CLI
  uses: metaplay-shared/github-workflows/setup-cli@v0
  with:
    login: false

- name: Run integration tests
  run: metaplay test integration
  working-directory: ./MyProject
```

#### Inputs

| Input         | Description                                                        | Required | Default  |
|---------------|--------------------------------------------------------------------|----------|----------|
| `version`     | Metaplay CLI version to install                                    | No       | `latest` |
| `login`       | Whether to authenticate to Metaplay cloud after installing the CLI | No       | `true`   |
| `credentials` | Metaplay machine user credentials (required when `login` is `true`)| No       |          |

### `setup-auth-cli` — Legacy authentication (deprecated)

> **Deprecated:** Upgrade to `setup-cli`. Follow the [Setup CI Pipeline](https://docs.metaplay.io/cloud-deployments/setup-ci-pipeline) guide to set up your new pipeline.

### Reusable workflows (deprecated)

> **Deprecated:** These workflows use the legacy `metaplay-auth` npm package. Follow the [Setup CI Pipeline](https://docs.metaplay.io/cloud-deployments/setup-ci-pipeline) guide to set up your new pipeline.

- **`build-deploy-server.yaml`** — Orchestrates a full build and deploy pipeline
- **`build-image.yaml`** — Builds and pushes a Docker image to the Metaplay registry
- **`deploy-server.yaml`** — Deploys a server image using Helm
