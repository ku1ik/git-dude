# git-dude

git-dude is a simple git desktop notifier. It monitors git repositories in
current directory for new commits/branches/tags and shows desktop notification if
anything new arrived.

## How it works

It simply uses `git fetch` and parses its output to see what has changed. Then it
formats new commit messages with `git log` and shows desktop notification with
`notify-send` / `kdialog` (Linux) or `growlnotify` (OSX). All of this in infinite loop.

## How does it look

Fedora:

![git-dude on Fedora](https://github.com/downloads/sickill/git-dude/git-dude-fedora-shot.png)

Ubuntu:

![git-dude on Ubuntu](https://github.com/downloads/sickill/git-dude/git-dude-ubuntu-shot.png)

OSX:

![git-dude on Mac OSX](https://github.com/downloads/sickill/git-dude/git-dude-osx-shot.png)

## Requirements

On Linux:

* `notify-send` on Gnome (Fedora: _libnotify_ package, Ubuntu: _libnotify-bin_ package)
* `kdialog` on KDE (included in KDE)

On OSX:

* `growlnotify`, from [Growl Extras](http://growl.info/extras.php#growlnotify)
  (Homebrew: _growlnotify_ package)

## Installation

    $ curl -skL https://github.com/sickill/git-dude/raw/master/git-dude >~/bin/git-dude
    $ chmod +x ~/bin/git-dude

\* Make sure `~/bin` is in your `$PATH` or put `git-dude` script somewhere else
on your `$PATH`.

## Usage

git-dude iterates over repositories that live inside _the dude directory_. This
directory is nothing more than container for cloned repositories of projects
you want to watch.  Name it like you want, here for example we use
_~/.git-dude_:

    $ mkdir ~/.git-dude
    $ cd ~/.git-dude

Clone some repositories:

    $ git clone --mirror https://github.com/joelthelion/autojump.git
    $ git clone --mirror git://github.com/pyromaniac/hoof.git

I recommend `git clone --mirror` - it doesn't checkout working directory so it
saves some disk space for bigger projects.

Symlinked repositories work too. This way you can monitor already cloned
projects:

    $ ln -s ~/code/tmuxinator .

Now run this to monitor _pwd_:

    $ git dude

You can also pass directory name as first argument to specify which directory
to monitor instead of _pwd_.

    $ git dude ~/watched-repos

This way you can have multiple _dude directories_ each being monitored by
separate git-dude process.

## Configuration

### Global

Set how often git-dude should check for changes (in seconds, default: 60):

    $ git config --global dude.interval 30

Set path to icon used by desktop notifications (default: none):

    $ git config --global dude.icon ~/.git-dude/github_32.png

### Per-repository

Set path to icon used by desktop notifications for this repository (default:
taken from global setting):

    $ git config dude.icon ~/.git-dude/dm-core/datamapper.png

## Author

Marcin Kulik (http://ku1ik.com/ | @sickill)
