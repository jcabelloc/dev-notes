# Jekyll Quickstart
Reference: https://jekyllrb.com/docs/

* After Installing a full Ruby development environment and installing Jekyll and bundler gems

* Create a new Jekyll site at ./myblog
```bash
$ jekyll new myblog
```

* Change into your new directory
```bash
$ cd myblog
```

* Build the site and make it available on a local server
```bash
bundle exec jekyll serve
```
* Now browse to http://localhost:4000

### Add Google Analytics to the web site (jcabelloc.github.io)
* Add the following line to the _config.yml file
```
# Google Analytics
google_analytics: UA—XXXXXXXX-X
```