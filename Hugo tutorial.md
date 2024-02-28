https://themes.gohugo.io/themes/docura/
```
hugo new site newsite
cd newsite
git init
git submodule add https://github.com/docura/docura.git themes/docura
rm hugo.toml && cp themes/docura/hugo.yaml .
hugo server
```
https://staticmania.com/blog/deploy-your-hugo-website-on-github-pages
