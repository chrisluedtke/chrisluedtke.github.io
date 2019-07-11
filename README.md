### Local Jekyll configuration (Windows)

On Windows, install WSL with Ubuntu. Then activate the WSL terminal with `bash`. Follow instructions here: https://jekyllrb.com/docs/installation/windows/

I also had to follow the instruction here:
https://jekyllrb.com/docs/troubleshooting/#no-sudo

I also had to follow these steps, from here: https://garfbradaz.github.io/blog/2018/12/12/Setting-up-Github-Pages-Jekyll-and-using-Windows-Subsystem-for-Linux.html
```
sudo apt-get install libpng-dev
sudo apt-get install --reinstall zlibc zlib1g zlib1g-dev
```

Navigate to the root of this repo, then create a `GemFile` file with:
```
source 'https://rubygems.org'
gem 'github-pages', group: :jekyll_plugins
```

```
bundle install
```

Get syntax styling `.css`
```
rougify style github > assets/css/syntax.css  
```

```
bundle exec jekyll serve
```

### Theme

Created from: [BlackrockDigital/startbootstrap-clean-blog](https://github.com/BlackrockDigital/startbootstrap-clean-blog)

Template link: [startbootstrap.com/template-overviews/clean-blog](https://startbootstrap.com/template-overviews/clean-blog)
