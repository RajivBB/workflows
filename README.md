# 01Cloud Action Workflows

## Required Action Variables
```
REGISTRY_URL=
KUBECONFIG=
GOOGLE_CHAT_WEBHOOK=
```

## Sample Workflow for Web Service
```
name: Development
on:
  workflow_dispatch:
  push:
    branches:
      - develop
jobs:
  lint:
    name: Lint
    secrets: inherit
    uses: berrybytes/actions/.github/workflows/lint.yaml@main
  test:
    name: Test
    needs: Lint
    secrets: inherit
    uses: berrybytes/actions/.github/workflows/test.yaml@main
  build:
    name: Build
    needs: test
    secrets: inherit
    uses: berrybytes/actions/.github/workflows/build.yaml@main
    with:
      build-env: dev
  deploy:
    name: Deploy
    needs: build
    secrets: inherit
    uses: berrybytes/actions/.github/workflows/deploy.yaml@main
    with:
      build-env: dev
      k8s-namespace: 01cloud
      k8s-deployment: api
      k8s-container: api
  notify:
    name: Notify
    needs: deploy
    secrets: inherit
    uses: berrybytes/actions/.github/workflows/notify.yaml@main

```
