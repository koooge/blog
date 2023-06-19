[![Netlify Status](https://api.netlify.com/api/v1/badges/71845e36-dbab-43e6-bf95-81c88aa0959f/deploy-status)](https://app.netlify.com/sites/koooge/deploys)

# blog
Wannabe engineer's blog https://koooge.com/

## Setting
### Netlify
Build command: `$ hugo -t @koooge/hugo-theme-geppaku --minify`
Publish directory: `public`

## Usage
### Create a new post
```sh
$ hugo new posts/<yyyy>/<mm>/<dd>_<title>/index.md
```

### preview
```sh
$ hugo server --buildDrafts --bind=0.0.0.0 --watch
```


### Update theme
```sh
$ git submodule foreach git pull origin main
```
