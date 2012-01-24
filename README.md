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

### Homebrew

Git-dude can be installed with the following command:

`brew install https://raw.github.com/gist/1289314/git-dude.rb --HEAD`

The homebrew formula lives [here](https://gist.github.com/1289314).

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

Set custom notification command (`$TITLE`, `$DESCRIPTION` and `$ICON_PATH`
environment variables are set when invoking notification command):

    $ git config --global dude.notify-command 'gntp-send "$TITLE" "$DESCRIPTION" "$ICON_PATH"'
    $ git config --global dude.notify-command 'echo -e "$TITLE\n\n\n$DESCRIPTION" | espeak --stdin -k20 -ven+12'

### Per-repository

Set path to icon used by desktop notifications for this repository (default:
taken from global setting):

    $ git config dude.icon ~/.git-dude/dm-core/datamapper.png

Tell git-dude to ignore specific repository (if you want to _unmonitor_ it):

    $ git config dude.ignore true

## Running git-dude in background

Currently you must have git-dude running in its own dedicated tab/screen. To avoid
this minor inconvenience, append the following function to your ```~/.bashrc```

```
# Git dude repo monitoring service
# arg1: "stop" or "start"
function gd(){
    val="$1"

    if [ $val == "start" ]; then
        git dude ~/.git-dude &>/dev/null &
        printf "git-dude started\n"
    elif [ $val == "stop" ];    then
        ps aux | grep 'git[ -]dude' | awk '{print $2}' | xargs sudo kill -9
        printf "git-dude stopped\n"
    else
        printf "$val not valid. Use start/stop\n"
    fi
}
```

You can then start git-dude via

    $ gd start

Similarly, you can stop git-dude via

    $ gd stop

## Running git-dude automatically on login

In addition to running the git-dude process in the background, we can also
set git-dude to start automatically once you log in. We will do so by implementing
OS X's [Login Hooks](http://support.apple.com/kb/HT2420). 

Change to your ```/Library``` directory and make a ```Hooks``` directory.

    $ cd /Library
    $ mkdir Hooks
    $ cd Hooks

Now create a file called ```login``` in your favorite text editor.

    $ vim login

Paste the following into the ```login``` file and save.

```
#!/bin/bash
/path/to/git dude /Users/YourUser/.git-dude &>/dev/null &
```

You can find your ```/path/to/git``` via 

    $ which git

**NOTE**: It's important that you reference absolute paths in your ```login``` script.

Now we need to make this ```login``` file executable.

    $ chmod +x login

We now add the script to the login hook.

    $ sudo defaults write com.apple.loginwindow LoginHook /Library/Hooks/login

Note that this modifies the ```/var/root/Library/Preferences/com.apple.loginwindow``` file if you ever want to undo it.

Feel free to log out and log back in. To check and see if the git-dude process is 
running, you can search for the process via

    $ ps aux | grep "git-dude"

## Author

Marcin Kulik (http://ku1ik.com/ | @sickill)
