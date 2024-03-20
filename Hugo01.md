# Creating new site
```
hugo new site portfolio
```
![](_resources/Pasted%20image%2020240307112724.png)
```
#hugo.toml
baseURL = 'https://example.org/'
languageCode = 'en-us'
title = 'My New Hugo Site'
```
## Layout
![](_resources/Pasted%20image%2020240307130949.png)
There will be separate layout for homepage, blog post pages etc.
![](_resources/Pasted%20image%2020240307132603.png)
`.Site` means it pulls data of `Title` from the site's data rather than from the page's data.
`.Content` doesn't uses `.Site` prefix because this data is related to the page.
## Context
![](_resources/Pasted%20image%2020240307133133.png)
## Content
For the site's home page, Hugo will look for the content in a file named `_index.md` in the `content` directory.
![](_resources/Pasted%20image%2020240307133602.png)
## hugo server
```
hugo server
```
It'll start a hugo developmental server.
Hugo's development server automatically reloads files when they change.
## Adding another page to the site
`hugo new about.md`
```
+++
title = 'About'
date = 2024-03-08T14:27:06+05:45
draft = true
+++
```
This created a file in `content/about.md`.
This needs different layout. `about.md` is a single page(as per Hugo). So, you need a "single page" layout to display it.
Hugo won't display the page till `draft=false`.
# HTML
Just type `hugo` without any options.
There will be a directory called `public` which will contain html files.
![](_resources/Pasted%20image%2020240308143529.png)
29
