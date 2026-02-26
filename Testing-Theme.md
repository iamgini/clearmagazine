# Testing ClearMagazine Theme

## Update and tag the theme

Theme repo (clearmagazine)

Do your edits and test locally (with the env override).

Commit the changes.

```shell
git add -A
git commit -m "Tweak cards, fix center shortcode, etc."
git push
```

Create an annotated tag on that commit and push both commits and tags.

```shell
git tag -a v0.1.3 -m "v0.1.3: card tweaks + shortcode fix"
git push --tags
```

That guarantees the tag points to the exact code you tested.

Don’t tag before committing; you’ll end up tagging the previous commit.


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

Export environment variable for local folder testing

```shell
# use the correct path where you cloned theme repo
export HUGO_MODULE_REPLACEMENTS="github.com/iamgini/clearmagazine->../../clearmagazine"
```

Note: You don’t need the `[module] [[module.imports]]` block in `hugo.toml` if you’re using `config/_default/module.toml`. It’s redundant.

---

## SEO

Make sure your params/front-matter are set

In config/_default/params.toml (or hugo.toml):

```shell
[params]
description    = "Techbeatly — practical guides on DevOps, automation, and cloud."
author         = "Techbeatly"
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