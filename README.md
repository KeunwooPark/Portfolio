# Portfolio

My portfolio website

## Install

Install [Ruby and Jekyll](https://jekyllrb.com/docs/installation/).

## How to Add a Post?

1. Create a file in \_posts. Images should be in assets/img/{project name}/.
2. Update `last_updated` field in `index.html`.

## Run Jekyll

```shell
$ bundle exec jekyll serve
```

## Troublshooting

### Could not find rb-fsevent-0.10.4 in any of the sources (Bundler::GemNotFound)

```shell
bundle install
bundle update
bundle exec jekyll build
```
