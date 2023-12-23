+++
title = "This website"
weight = 300
+++

The current iteration of this project is a collection of markdown
files built into a static website using zola. I'm using the [zola
easydocs theme](https://github.com/poudel/zola_easydocs_theme/) with
some modifications.


## Folded code blocks

I have a habit of including code snippets and outputs of commands in
my notes. Sometimes they can get quite long and make for a longer
scrolling. The solution was quite easy: a shortcode that can be used
to wrap the snippet inside a `<summary>` tag.

Saved the following inside `templates/shortcodes/folded.md`.

```html
<details>
    <summary>{{ title }}</summary>
    {{ body | markdown }}
</details>
```

Now I can simply use this as follows

```txt
{%/* folded(title="csv to datasette") */%}
<folded-content-goes-here>
{%/* end */%}
```

One added bonus is that I can fold any markdown content, not just code
blocks.


## Github action


[zbrox/zola-deploy-action](https://github.com/marketplace/actions/zola-deploy)
didn't work for me so I modified Github's default suggestion for a
Hugo-based workflow to work for Zola.

{% folded(title="build.yml") %}
``` yaml
name: Deploy to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches: ["main"]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
# However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
concurrency:
  group: "pages"
  cancel-in-progress: false

# Default to bash
defaults:
  run:
    shell: bash

jobs:
  # Build job
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.114.0
    steps:
      - name: Install Zola
        uses: taiki-e/install-action@v2
        with:
          tool: zola@0.17.1
      - name: Checkout
        uses: actions/checkout@v3
        with:
          submodules: recursive
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v3

      - name: Build with Zola
        run: zola build
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v2
        with:
          path: ./public

  # Deployment job
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2
```
{% end %}

In summary, this builds the page, uploads the output `./public`
directory as an artifact and then deploys it using the
`actions/deploy-pages` action.
