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

On 2025-02-01, my build failed because I was using an older version of
an action. Used that ocassion to replace the old multi step workflow
with a simplified one using the `shalzz/zola-deploy-action` action.

{% folded(title="build.yml") %}
``` yaml
name: Deploy to Pages

on: 
 push:
  branches:
   - main

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
# Cancel running workflow in favor of new ones.
concurrency:
  group: "pages"
  cancel-in-progress: true

jobs:
  build:
    name: Publish site
    runs-on: ubuntu-latest
    steps:
    - name: Checkout main
      uses: actions/checkout@v4
      with:
        submodules: recursive

    - name: Build and deploy
      uses: shalzz/zola-deploy-action@v0.19.2
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```
{% end %}

In summary, this builds the page and pushes it to the `gh-pages`
branch which triggers a page deployment. I had to set the page to be
deployed from that branch instead of main in the settings and also add
write permission to the github token.

Previously, I had a workflow that pushed the built artifact and used
`actions/deploy-pages`. This is much simpler.
