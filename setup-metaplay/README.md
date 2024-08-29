
# Overview

Github composite action for installing the Metaplay CLI within your own Github Action workflow.

Using Github Actions is the simplest way to get started with deploying your game server into
the Metaplay managed cloud.

# Usage

For instructions on using this composite action, go to the [Metaplay Portal](https://portal.metaplay.dev),
navigate to the environment you want to deploy into and open the 'Deploy Server' tab to find
a template that has your project details filled in already.

Below is an example of how this could look. If you choose to use this, do update your project
details, check the paths, and update the latest Helm chart version.

```yaml
jobs:
  # Build the server and deploy into the cloud
  build-and-deploy-server:
    runs-on: ubicloud
    steps:
      # check out repository with helm values
      - name: Checkout repo
        uses: actions/checkout@v4
        with:
          show-progress: false # suppress the noisy progress status output

      - name: Setup Metaplay CLI
        uses: metaplay/github-actions/setup-metaplay@composites
        with:
          version: 'next'
          credentials: ${{ secrets.METAPLAY_CREDENTIALS}}

      - name: Check access to target environment
        run: metaplay-auth get-environment idler-develop

      - name: Build server image
        run: |
          docker buildx build --pull \
            --platform linux/amd64 \
            -t gameserver:$GITHUB_SHA \
            -f MetaplaySDK/Dockerfile.server \
            --build-arg PROJECT_ROOT="Samples/Idler" \
            --build-arg COMMIT_ID=$GITHUB_SHA \
            --build-arg BUILD_NUMBER=$GITHUB_RUN_NUMBER \
            .

      - name: Push server image
        run: metaplay-auth push-docker-image idler-develop gameserver:$GITHUB_SHA

      - name: Deploy server
        run: |
          metaplay-auth deploy-server idler-develop \
            $GITHUB_SHA \
            --values=Samples/Idler/Backend/Deployments/develop-server.yaml \
            --helm-chart-version=0.6.3
```
