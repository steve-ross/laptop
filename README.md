Laptop
======

Laptop is a script to set up a macOS laptop for web and mobile development.

It can be run multiple times on the same machine safely.
It installs, upgrades, or skips packages
based on what is already installed on the machine.

Requirements
------------

We support:

* macOS Mavericks (10.9)
* macOS Yosemite (10.10)
* macOS El Capitan (10.11)
* macOS Sierra (10.12)
* macOS High Sierra (10.13)
* macOS Mojave (10.14)
* macOS Catalina (10.15)

Older versions may work but aren't regularly tested.
Bug reports for older versions are welcome.

Install
-------

Download the script:

```sh
curl --remote-name https://raw.githubusercontent.com/steve-ross/laptop/master/mac
```

Review the script (avoid running scripts you haven't read!):

```sh
less mac
```

Execute the downloaded script:

```sh
sh mac 2>&1 | tee ~/laptop.log
```

Optionally, review the log:

```sh
less ~/laptop.log
```


Debugging
---------

Your last Laptop run will be saved to `~/laptop.log`.
Read through it to see if you can debug the issue yourself.
If not, copy the lines where the script failed into a
[new GitHub Issue](https://github.com/steve-ross/laptop/issues/new) for us.
Or, attach the whole log file as an attachment.

What it sets up
---------------

macOS tools:

* [Homebrew] for managing operating system libraries.

[Homebrew]: http://brew.sh/

macoOS Applications:

- [1Password] password manager
- [Adobe Creative Cloud] for installing Adobe apps
- [Dropbox] for cloud storage
- [Env Key] to manage dotfiles and environment variables
- [Google Chrome], the web browser
- [GitKracken] to better visualize and manage git
- [Harvest] for time tracking
- [iTerm2] to replace the default console app
- [Moom] for arranging windows
- [No IP Dynamic Update Client] for managing dynamic IPs
- [Skitch] for screenshot annotations
- [Slack] for team / collaborative communications
- [Spotify] for music
- [Transmit] for all things remote storage (FTP etc)
- [Visual Studio Code] text editor for programming


[1Password]: https://1password.com/
[Adobe Creative Cloud]: http://adobe.com
[Dropbox]: http://dropbox.com
[Env Key]: https://www.envkey.com/
[Google Chrome]: https://www.google.com/chrome/
[GitKracken]: https://www.gitkraken.com/
[Harvest]: https://getharvest.com
[iTerm2]: https://iterm2.com/
[Moom]: https://manytricks.com/moom/
[No IP Dynamic Update Client]: https://www.noip.com/download?page=mac
[Skitch]: https://evernote.com/products/skitch
[Slack]: https://slack.com
[Spotify]: https://spotify.com
[Transmit]: https://panic.com/transmit/
[Visual Studio Code]: https://code.visualstudio.com/


Unix tools:

- [Autojump] to move between directories
- [Direnv] and [Direnv Helpers] to auto-layout projects 
- [Fish] as your shell
  - addons: [Starship], [Direnv], [Fira Code]
- [Terminal Notifier] long running shell process alerts/notifications
- [Git] for version control
- [OpenSSL] for Transport Layer Security (TLS)
- [RCM] for managing company and personal dotfiles
- [The Silver Searcher] for finding things in files
- [Tmux] for saving project state and switching between projects
- [Watchman] for watching for filesystem events

[Fira Code]: https://github.com/tonsky/FiraCode
[Autojump]: https://github.com/wting/autojump
[Universal Ctags]: https://ctags.io/
[Git]: https://git-scm.com/
[OpenSSL]: https://www.openssl.org/
[RCM]: https://github.com/thoughtbot/rcm
[The Silver Searcher]: https://github.com/ggreer/the_silver_searcher
[Tmux]: http://tmux.github.io/
[Watchman]: https://facebook.github.io/watchman/
[Fish]: https://fishshell.com/
[Starship]: https://starship.rs/
[Direnv]: https://direnv.net/
[Direnv Helpers]: https://github.com/steve-ross/direnv-helpers
[Terminal Notifier]: https://github.com/julienXX/terminal-notifier
[Git-Flow]: https://datasift.github.io/gitflow/IntroducingGitFlow.html
[Hub]: https://hub.github.com/

Heroku tools:

* [Heroku CLI] and [Parity] for interacting with the Heroku API

[Heroku CLI]: https://devcenter.heroku.com/articles/heroku-cli
[Parity]: https://github.com/thoughtbot/parity

GitHub tools:

- [Hub] for interacting with the GitHub API
- [Git-Flow] branching model

[Hub]: http://hub.github.com/

Image tools:

* [GraphicksMagick] for cropping and resizing images

Programming languages, package managers, and configuration:

- [Node Version Manager] for managing node versions
- [Node.js] and [npm], for running apps and installing JavaScript packages
- [Yarn] for managing JavaScript packages


[GraphicksMagick]: http://www.graphicsmagick.org/
[Node.js]: http://nodejs.org/
[npm]: https://www.npmjs.org/
[Node Version Manager]: https://github.com/nvm-sh/nvm
[Yarn]: https://yarnpkg.com/en/

Databases:

* [Mongo] for storing document data
* [Redis] for storing key-value data

[MongoDB]: http://www.mongodb.com/
[Redis]: http://redis.io/

It should take less than 15 minutes to install (depends on your machine).

Customize in `~/.laptop.local`
------------------------------

Your `~/.laptop.local` is run at the end of the Laptop script.
Put your customizations there.
For example:

```sh
#!/bin/sh

brew bundle --file=- <<EOF
brew "Caskroom/cask/dockertoolbox"
brew "go"
brew "ngrok"
brew "watch"
EOF

default_docker_machine() {
  docker-machine ls | grep -Fq "default"
}

if ! default_docker_machine; then
  docker-machine create --driver virtualbox default
fi

default_docker_machine_running() {
  default_docker_machine | grep -Fq "Running"
}

if ! default_docker_machine_running; then
  docker-machine start default
fi

fancy_echo "Cleaning up old Homebrew formulae ..."
brew cleanup
brew cask cleanup

if [ -r "$HOME/.rcrc" ]; then
  fancy_echo "Updating dotfiles ..."
  rcup
fi
```

Write your customizations such that they can be run safely more than once.
See the `mac` script for examples.

Laptop functions such as `fancy_echo` and
`gem_install_or_update`
can be used in your `~/.laptop.local`.

See the [wiki](https://github.com/steve-ross/laptop/wiki)
for more customization examples.

Contributing
------------

Edit the `mac` file.
Document in the `README.md` file.
Follow shell style guidelines by using [ShellCheck] and [Syntastic].

```sh
brew install shellcheck
```

[ShellCheck]: http://www.shellcheck.net/about.html
[Syntastic]: https://github.com/scrooloose/syntastic

Thank you, [contributors]!

[contributors]: https://github.com/steve-ross/laptop/graphs/contributors
[contributors]: https://github.com/thoughtbot/laptop/graphs/contributors

By participating in this project,
you agree to abide by the thoughtbot [code of conduct].

[code of conduct]: https://thoughtbot.com/open-source-code-of-conduct

License
-------

Laptop is © 2011-2020 thoughtbot, inc.
It is free software,
and may be redistributed under the terms specified in the [LICENSE] file.

[LICENSE]: LICENSE
