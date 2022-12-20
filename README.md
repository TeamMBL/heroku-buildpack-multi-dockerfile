# Heroku Multi Docker buildpack

_tl;dr_ -- For a monorepository which may need multiple docker files

```
$ heroku create -a example-1
$ heroku create -a example-2
$ heroku buildpacks:add -a example-1 heroku-community/multi-procfile
$ heroku buildpacks:add -a example-2 heroku-community/multi-procfile
$ heroku config:set -a example-1 PROCFILE=Procfile
$ heroku config:set -a example-2 PROCFILE=backend/Procfile
$ git push https://git.heroku.com/example-1.git HEAD:master
$ git push https://git.heroku.com/example-2.git HEAD:master
```

When example-1 builds, it'll copy Dockerfile into /app/Dockerfile, and when example-2 builds, it'll copy backend/Dockerfile to /app/Dockerfile. For example-2, the process types available for you to scale up will be the ones referenced (originally) in backend/Dockerfile.

## Pipelines

Only **builds** will set the proper Dockerfile. If you use [Heroku Pipelines](https://devcenter.heroku.com/articles/pipelines), then promoting a slug downstream will not trigger a build, and therefore will not look at the environment variable and act accordingly. Make sure that the proper Dockerfile is referenced all the way upstream to the first stage that builds.

