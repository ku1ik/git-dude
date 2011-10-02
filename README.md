# git-dude

git-dude is a simple git desktop notifier. It monitors git repositories in
given directory for new commits and branches and shows desktop notification if
anything new arrived.

## How it works

It simply uses `git fetch` and parses its output to see what changed. Then it
formats new commit messages with `git log` and shows desktop notification with
`notify-send` (Linux) or `growlnotify` (OSX). All of this in infinite loop.

## Installation

    $ curl -skL https://github.com/sickill/git-dude/raw/master/git-dude >~/bin/git-dude
    $ chmod +x ~/bin/git-dude

## Usage

git-dude expects you have a dedicated directory that contains git repositories
(as direct children).

    $ mkdir ~/.git-dude
    $ cd ~/.git-dude
    $ git clone --mirror some-repo-url
    $ git clone --mirror other-repo-url

Symlinked repositories work too. This way you can monitor already cloned
projects:

    $ cd ~/.git-dude
    $ ln -s ~/code/existing-repo

Now, from _dude_ directory run:

    $ cd ~/.git-dude
    $ git dude

You can also pass directory name as first argument to specify which directory
to monitor instead of _pwd_.

    $ cd ~
    $ git dude ~/.git-dude

This way you can have multiple _dude_ directories each being monitored by
separate git-dude process.

## Configuration

Set how often git-dude should check for changes (in seconds, default: 60):

    $ git config --global dude.interval 30

Set path to icon used by desktop notifications (default: none):

    $ git config --global dude.icon ~/.git-dude/github_32.png

## Author

Marcin Kulik (http://ku1ik.com/ | @sickill)
