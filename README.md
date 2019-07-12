## Local Jekyll Setup on Windows

To develop and preview my website locally, I followed the [WSL Jekyll installation guide](https://jekyllrb.com/docs/installation/windows/#installation-via-bash-on-windows-10):

1. Install WSL and Ubuntu from Microsoft Store
2. Start a WSL terminal
    ```
    bash
    sudo apt-get update -y && sudo apt-get upgrade -y
    sudo apt-add-repository ppa:brightbox/ruby-ng
    sudo apt-get update
    sudo apt-get install ruby2.5 ruby2.5-dev build-essential dh-autoreconf
    gem update
    gem install jekyll bundler
    ```
3. If you receive an error at `gem update` or `gem install jekyll bundler`, add the following lines to `.bashrc` and try those commands again. See: [Running Jekyll as Non-Superuser (no sudo!)](https://jekyllrb.com/docs/troubleshooting/#no-sudo).
    ```
    # Ruby exports

    export GEM_HOME=$HOME/gems
    export PATH=$HOME/gems/bin:$PATH
    ```
4. Prior to `bundle install` below, I also had to issue these commands (from [this guide](https://garfbradaz.github.io/blog/2018/12/12/Setting-up-Github-Pages-Jekyll-and-using-Windows-Subsystem-for-Linux.html))
    ```
    sudo apt-get install libpng-dev
    sudo apt-get install --reinstall zlibc zlib1g zlib1g-dev
    ```
5. Navigate to the root of this repo, then create a `GemFile` file containing:
    ```
    source 'https://rubygems.org'
    gem 'github-pages', group: :jekyll_plugins
    ```
6. create the dependencies
    ```
    bundle install
    ```
7. (Optional) get syntax styling `.css`
    ```
    rougify style github > assets/css/syntax.css  
    ```
8. Boot the "server"
    ```
    bundle exec jekyll serve
    ```

## Theme Credits

Created from: [BlackrockDigital/startbootstrap-clean-blog](https://github.com/BlackrockDigital/startbootstrap-clean-blog)

Template link: [startbootstrap.com/template-overviews/clean-blog](https://startbootstrap.com/template-overviews/clean-blog)
