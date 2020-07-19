# Personal Blog

This is my personal blog made with Hugo, a static site generator for Go. I'm using the Tranquil Peak theme.

https://gohugo.io/  
https://github.com/kakawait/hugo-tranquilpeak-theme  

## Directions

Install Hugo
- https://gohugo.io/getting-started/quick-start/

Install the Tranquil Peak Theme
- cd blog
- git init
- git submodule add https://github.com/kakawait/hugo-tranquilpeak-theme themes/tranquilpeak
- echo "theme = \\"tranquilpeak\\"" > config.toml

Run the development server
- hugo server -D
