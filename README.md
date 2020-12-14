# Before anything

1. `git clone https://github.com/HLTCHKUST/hltchkust.github.io.git`
2. `git fetch`
3. `git checkout source`

# To add new blog

Create a new markdown file under `_posts`, follow the format of previous ones.

# To add an announcement to *News*

Create a new markdown file under `_news`, follow the format of previous ones.

# To add a new publication

Just add your bibliography to the `_bibliography/papers.bib`. If you want to show it in the *Selected publications*, add the `selected={true}` field.

# Deployment

To deploy new changes, just

`./bin/deploy --user`

(no need to push anything)

Wait a few seconds, and refresh!

# For local development

Make sure you have [Ruby](https://www.ruby-lang.org/en/downloads/) and [Bundler](https://bundler.io/) installed on your system (hint: for ease of managing ruby gems, consider using [rbenv](https://github.com/rbenv/rbenv))

```bash
bundle install
bundle exec jekyll serve
```

Then, a localhost is established at port 4000.
