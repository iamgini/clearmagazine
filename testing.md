

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