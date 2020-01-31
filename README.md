## Local Jekyll Setup on Windows

1. [Install WSL](https://docs.microsoft.com/en-us/windows/wsl/install-win10) and Ubuntu from the Microsoft Store
2. Followed the [WSL Jekyll installation guide](https://jekyllrb.com/docs/installation/windows/#installation-via-bash-on-windows-10)
3. If you receive a permission error at `gem update` or `gem install jekyll bundler`, locate `.bashrc`:
    ```bash
    nano C:\Users\USERNAME\AppData\Local\Packages\{DIST}\LocalState\rootfs\home\{LINUXUSER}\.bashrc
    ```
    *Alternatively:*
    ```bash
    nano ~/.bashrc
    ```
    Add the following lines (see: [Running Jekyll as Non-Superuser (no sudo!)](https://jekyllrb.com/docs/troubleshooting/#no-sudo)
    ```
    # Ruby exports

    export GEM_HOME=$HOME/gems
    export PATH=$HOME/gems/bin:$PATH
    ```
    Restart the terminal, or reload `.bashrc`
    ```bash
    . ~/.bashrc
    ```
    Resume the [WSL Jekyll installation guide](https://jekyllrb.com/docs/installation/windows/#installation-via-bash-on-windows-10).

## Local GitHub Pages Setup on Windows

1. Prior to `bundle install` below, I also had to issue these commands (see [this guide](https://garfbradaz.github.io/blog/2018/12/12/Setting-up-Github-Pages-Jekyll-and-using-Windows-Subsystem-for-Linux.html))
    ```
    sudo apt-get install libpng-dev
    sudo apt-get install --reinstall zlibc zlib1g zlib1g-dev
    ```
2. Install dependencies as specified in this repo's `GemFile` file
    ```
    bundle install
    ```
3. (Optional) get syntax styling `.css`
    ```
    rougify style github > assets/css/syntax.css  
    ```
4. Boot the local server
    ```
    bundle exec jekyll serve
    ```

## Theme Credits

Created from: [BlackrockDigital/startbootstrap-clean-blog](https://github.com/BlackrockDigital/startbootstrap-clean-blog)

Template link: [startbootstrap.com/template-overviews/clean-blog](https://startbootstrap.com/template-overviews/clean-blog)
