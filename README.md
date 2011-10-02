# git-dude

git-dude is a simple git desktop notifier. It monitors git repositories in
given directory for new commits and branches and shows desktop notification if
anything new arrived.

## Installation

    $ curl -skL https://github.com/sickill/git-dude/raw/master/git-dude >~/bin/git-dude
    $ chmod +x ~/bin/git-dude

## Usage

    git-dude expects you have a directory that contains git repositories (as
direct children).

    $ mkdir ~/.git-dude
    $ cd ~/.git-dude
    $ git clone --mirror some-repo-url
    $ git clone --mirror other-repo-url
    $ git dude

You can pass directory as first argument to specify which dir contains
repositories (instead of pwd).

    $ git dude ~/.git-dude

## Configuration

    $ git config --global dude.interval 30

## Author

Marcin Kulik (@sickill)
