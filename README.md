# Website

[knowledge base](https://dominuszagare.github.io/dominuszagare-knowledge-base/docs/intro)

This website is built using [Docusaurus 2](https://docusaurus.io/), a modern static website generator.

All the documentation is written in Markdown and located in the `docs` directory.
[Markdown guide](https://www.markdownguide.org/basic-syntax/)

### Installation

```
$ yarn
```

### Local Development

```
$ yarn start
```

This command starts a local development server and opens up a browser window. Most changes are reflected live without having to restart the server.

### Continuous Deployment

This site is deployed using GitHub Actions by creating a workflow file in the `.github/workflows` directory. The workflow file is named `deploy.yml` and is configured to deploy the site to GitHub Pages.

```yml
# Simple workflow for deploying static content to GitHub Pages
name: Deploy static content to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches: [main]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow one concurrent deployment
concurrency:
  group: "pages"
  cancel-in-progress: true

jobs:
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      # 👇 Build steps
      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 16.x
          cache: yarn
      - name: Install dependencies
        run: yarn install --frozen-lockfile --non-interactive
      - name: Build
        run: yarn build
      # 👆 Build steps
      - name: Setup Pages
        uses: actions/configure-pages@v1
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v1
        with:
          # 👇 Specify build output path
          path: build
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v1
```


