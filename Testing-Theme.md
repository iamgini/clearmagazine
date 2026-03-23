# Testing ClearMagazine Theme

- [Testing ClearMagazine Theme](#testing-clearmagazine-theme)
  - [Step 1 — Edit \& test theme locally (no tagging yet)](#step-1--edit--test-theme-locally-no-tagging-yet)
  - [Step 2 — Happy with changes? Commit \& tag the theme](#step-2--happy-with-changes-commit--tag-the-theme)
  - [Now in the site directory](#now-in-the-site-directory)
  - [How to test theme in your site as Hugo Modules](#how-to-test-theme-in-your-site-as-hugo-modules)
  - [SEO](#seo)
  - [Troubleshooting](#troubleshooting)
    - [github.com/gethugothemes/hugo-modules/pwa not found](#githubcomgethugothemeshugo-modulespwa-not-found)
  - [Clean Cache](#clean-cache)
  - [Fixing Theme issues](#fixing-theme-issues)
  - [Per-site migration checklist](#per-site-migration-checklist)
    - [1. Git — sync staging with main first](#1-git--sync-staging-with-main-first)
    - [2. Clean up module state](#2-clean-up-module-state)
    - [3. Update `config/_default/module.toml`](#3-update-config_defaultmoduletoml)
    - [4. Fix .gitignore](#4-fix-gitignore)
    - [5. Generate and commit hugo\_stats.json](#5-generate-and-commit-hugo_statsjson)
    - [6. Test locally](#6-test-locally)
    - [7. Commit everything](#7-commit-everything)
    - [8. Verify staging deploy](#8-verify-staging-deploy)
    - [9. Merge to main when happy](#9-merge-to-main-when-happy)


## Step 1 — Edit & test theme locally (no tagging yet)
In your site repo, set the local replacement so Hugo uses your local theme folder directly:

```shell
# use the correct path where you cloned theme repo
export HUGO_MODULE_REPLACEMENTS="github.com/iamgini/clearmagazine->../../clearmagazine"
# export HUGO_MODULE_REPLACEMENTS="github.com/iamgini/clearmagazine->../clearmagazine"

hugo server --disableFastRender --cleanDestinationDir --environment development
```

Then run the site:

```shell
npm install

$ hugo server --disableFastRender --cleanDestinationDir 2>&1

$ hugo server --disableFastRender --cleanDestinationDir 2>&1 | grep -i environment
# or
hugo server --disableFastRender --cleanDestinationDir --environment development --watch
# or
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


## Now in the site directory

```shell
# 1. Update module.toml
# change version = "v0.1.6" to version = "v0.1.16"

# 2. Pull the new version
hugo mod get github.com/iamgini/clearmagazine@v0.1.16
hugo mod tidy


````

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

- https://gohugo.io/hugo-modules/use-modules/#replace
- https://gohugo.io/configuration/module/#replacements
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

### github.com/gethugothemes/hugo-modules/pwa not found

```shell
Error: command error: failed to load modules: module "github.com/gethugothemes/hugo-modules/pwa" not found in "/home/gmadappa/community/gineesh-com-content/themes/github.com/gethugothemes/hugo-modules/pwa"; either add it as a Hugo Module or store it in "/home/gmadappa/community/gineesh-com-content/themes".: module does not exist
```

Solution:
```shell
clearmagazine  $  hugo mod get github.com/gethugothemes/hugo-modules/pwa

clearmagazine  $  hugo mod tidy
```

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

## Per-site migration checklist

### 1. Git — sync staging with main first

```bash
git checkout staging
git fetch origin
git merge origin/main
git push origin staging
```

### 2. Clean up module state

```bash
rm -rf go.mod go.sum
hugo mod init github.com/iamgini/<site-name>-content
hugo mod get github.com/iamgini/clearmagazine@v0.1.20
# skip hugo mod tidy !
```

### 3. Update `config/_default/module.toml`

Replace everything with:

```ini
toml[hugoVersion]
extended = true
min = "0.144.0"

[[imports]]
path = "github.com/iamgini/clearmagazine"
version = "v0.1.20"
```

### 4. Fix .gitignore

Remove `hugo_stats.json` from `.gitignore`

### 5. Generate and commit hugo_stats.json

```bash
npm install
hugo --gc
```

### 6. Test locally

```bash
hugo server --disableFastRender --cleanDestinationDir
```

### 7. Commit everything

```bash
git add go.mod go.sum config/_default/module.toml .gitignore hugo_stats.json
git commit -m "Migrate to clearmagazine v0.1.20, clean module setup"
git push origin staging
```

### 8. Verify staging deploy

- Check Cloudflare Pages staging build log
- Confirm `npm install && hugo --gc --minify` is the build command
- Visually check the site

### 9. Merge to main when happy

```bash
git checkout main
git merge staging
git push origin main
```