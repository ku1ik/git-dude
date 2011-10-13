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

Windows:

![git-dude on Windows](https://github.com/downloads/mattn/git-dude/git-dude-windows-shot.png)

## Requirements

On Linux:

* `notify-send` on Gnome (Fedora: _libnotify_ package, Ubuntu: _libnotify-bin_ package)
* `kdialog` on KDE (included in KDE)
* [Growl for Linux](http://mattn.github.com/growl4linux) and [gntp-send](http://github.com/mattn/gntp-send) (ppa: [growl4linux](https://launchpad.net/~mattn/+archive/growl-for-linux), [gntp-send](https://launchpad.net/~mattn/+archive/gntp-send))

On OSX:

* `growlnotify`, from [Growl Extras](http://growl.info/extras.php#growlnotify)
  (Homebrew: _growlnotify_ package)

On Windows:

* [Growl for Windows](http://www.growlforwindows.com/gfw/default.aspx) and [growlnotify.exe](http://www.growlforwindows.com/gfw/help/growlnotify.aspx)

or

* [Growl for Linux](http://mattn.github.com/growl4linux) and [gntp-send.exe](http://github.com/mattn/gntp-send)

And [iconv.exe](http://gnuwin32.sourceforge.net/packages/libiconv.htm) is required.

## Installation

    $ curl -skL https://github.com/sickill/git-dude/raw/master/git-dude >~/bin/git-dude
    $ chmod +x ~/bin/git-dude

\* Make sure `~/bin` is in your `$PATH` or put `git-dude` script somewhere else
on your `$PATH`.

On windows, you should use `ssh-agent.exe` included in msysgit. save following batch file to git/bin as `ssh-askpass.bat`.

    @echo off
    set SSH_CMD_PATH=c:/progra~1/git/bin
    if "%1" == "-f" goto doit
    if "%1" == "-s" goto session
    if not "%SSH_AGENT_PID%" == "" goto end
    goto doit
    
    :session
    for /f "eol=; tokens=1,2 delims==;" %%1 in ('%SSH_CMD_PATH%/ssh-agent.exe') do (
     if "%%1" == "SSH_AUTH_SOCK" setx SSH_AUTH_SOCK %%2 & set SSH_AUTH_SOCK=%%2
     if "%%1" == "SSH_AGENT_PID" setx SSH_AGENT_PID %%2 & set SSH_AGENT_PID=%%2
    )
    goto bottom
    
    :doit
    for /f "eol=; tokens=1,2 delims==;" %%1 in ('%SSH_CMD_PATH%/ssh-agent.exe') do (
     if "%%1" == "SSH_AUTH_SOCK" set SSH_AUTH_SOCK=%%2
     if "%%1" == "SSH_AGENT_PID" set SSH_AGENT_PID=%%2
    )
    
    :bottom
    %SSH_CMD_PATH%/ssh-add
    :end

Open command prompt and type following.

    C:\github\myrepo>ssh-askpass.bat
    Enter passphrase for /c/docume~1/mattn/.ssh/id_rsa:
    Identity added: /c/docume~1/mattn/.ssh/id_rsa (/c/docume~1/mattn/.ssh/id_rsa)
    
    C:\github\myrepo>git dude

Note that this password session is temporary. If you want to use the session as permanent, type `ssh-askpass.bat -s`. You can use the session while you are logged in on windows.

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
