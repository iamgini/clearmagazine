# Testing ClearMagazine Theme

- [Testing ClearMagazine Theme](#testing-clearmagazine-theme)
  - [Step 1 — Edit \& test theme locally (no tagging yet)](#step-1--edit--test-theme-locally-no-tagging-yet)
  - [Step 2 — Happy with changes? Commit \& tag the theme](#step-2--happy-with-changes-commit--tag-the-theme)
  - [How to test theme in your site as Hugo Modules](#how-to-test-theme-in-your-site-as-hugo-modules)
  - [SEO](#seo)
  - [Troubleshooting](#troubleshooting)
  - [Clean Cache](#clean-cache)
  - [Fixing Theme issues](#fixing-theme-issues)


## Step 1 — Edit & test theme locally (no tagging yet)
In your site repo, set the local replacement so Hugo uses your local theme folder directly:

```shell
# use the correct path where you cloned theme repo
# export HUGO_MODULE_REPLACEMENTS="github.com/iamgini/clearmagazine->../../clearmagazine"
export HUGO_MODULE_REPLACEMENTS="github.com/iamgini/clearmagazine->../clearmagazine"
```

Then run the site:

```shell
npm install
hugo server --disableFastRender --cleanDestinationDir --environment development
```

## Step 2 — Happy with changes? Commit & tag the theme

Theme repo (clearmagazine)

```shell
cd clearmagazine/

git add -A
git commit -m "v0.1.X: describe what changed"
git tag -a v0.1.X -m "v0.1.X: describe what changed"
git push
git push --tags        # ← don't forget this
```

That guarantees the tag points to the exact code you tested.

Don’t tag before committing; you’ll end up tagging the previous commit.



see the latest push :(

the staging branch in cloudflare build!

https://c5c171db.gineesh-com-content.pages.dev/


## How to test theme in your site as Hugo Modules

1. Clone this project

```shell
git clone github.com/iamgini/clearmagazine
```

2. goto the hugo project path

```shell
cd /<path-to-your-directory>/<hugo-project>
```

3. Add and Pin the version in `config/_default/module.toml`:

```ini
.
.
.
[[imports]]
path = "github.com/iamgini/clearmagazine"
version = "v0.1.2"
.
.
.
```

4. Sync lockfiles

```shell
hugo mod get github.com/iamgini/clearmagazine@v0.1.12
hugo mod tidy
git add -A
git commit -m "Pin clearmagazine v0.1.8"
git push
```

5. Update theme `hugo.toml` and ensure NO theme is mentioned!

```ini
# theme = "hugoplate"
```
6. Local testing

**Method 1 - using Dev file**

Add `config/development/module.toml`.

```ini
[[module.replacements]]
path = "github.com/iamgini/clearmagazine"
replace = "../../clearmagazine"
```

```shell
$  tree config/
config/
├── _default
│   ├── languages.toml
│   ├── menus.en.toml
│   ├── module.toml
│   └── params.toml
└── development
    ├── module.toml
    └── server.toml

3 directories, 6 files
```

Hugo merges:
- `_default/module.toml`
- `development/module.toml`

Then test the site locally by passing environment.

```shell
$ hugo server --disableFastRender \
    --cleanDestinationDir \
    --environment development
```

**Method 2 - Using Environment variable**

Export environment variable for local folder testing

```shell
# use the correct path where you cloned theme repo
# export HUGO_MODULE_REPLACEMENTS="github.com/iamgini/clearmagazine->../../clearmagazine"
export HUGO_MODULE_REPLACEMENTS="github.com/iamgini/clearmagazine->../clearmagazine"
```

Note: You don’t need the `[module] [[module.imports]]` block in `hugo.toml` if you’re using `config/_default/module.toml`. It’s redundant.

---


## SEO

Make sure your params/front-matter are set

In config/_default/params.toml (or hugo.toml):

```shell
[params]
description    = "techbeatly — practical guides on DevOps, automation, and cloud."
author         = "techbeatly"
defaultImage   = "/images/default-og.jpg"
twitterSite    = "@techbeatly"
twitterCreator = "@iamgini"
```

On the page front-matter (e.g. content/ansible-windows/index.md):

```yaml
title: "Ansible Automation for Windows"
description: "Automate Windows with Ansible: WinRM setup, modules, and playbook patterns."
author: "Gineesh Madapparambath"
image: "/images/tb-uploads/2022/01/ansible-windows-techbeatly.png"
```

## Troubleshooting

Inside your project root:

```shell
rm -rf public
rm -rf resources
rm -rf go.sum
rm -rf go.mod
```
Now re-initialize modules fresh:

```shell
hugo mod init gineesh-com-content
hugo mod tidy
hugo mod get github.com/iamgini/clearmagazine@v0.1.6
hugo mod tidy
```

Then clean module cache completely:

```shell
hugo mod clean
```

Now re-initialize modules fresh:

```shell
hugo --gc --minify
```


---
hugo mod tidy
hugo --gc --minify

## Clean Cache

```shell
rm -rf public
rm -rf resources/_gen
rm -rf .hugo_build.lock
rm -rf .hugo_cache
rm -rf go.sum
rm -rf go.mod

hugo mod clean
rm -rf $HOME/.cache/hugo_modules
hugo mod tidy

hugo --cleanDestinationDir --gc
```

```shell
hugo server --disableFastRender --ignoreCache --gc
```

## Fixing Theme issues

```shell
# Full nuclear clean
rm -rf public
rm -rf resources/_gen
rm -rf .hugo_build.lock
rm -rf go.sum
rm -rf go.mod

# Re-init
$ hugo mod init github.com/iamgini/gineesh-com-content
$ hugo mod get github.com/iamgini/clearmagazine@v0.1.6
$ hugo mod tidy

# Clean module cache too
hugo mod clean
rm -rf $HOME/.cache/hugo_modules

```