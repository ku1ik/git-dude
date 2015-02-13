#!/bin/bash
#
# git-dude - Git commit notifier
# https://github.com/sickill/git-dude
#
# Copyright (C) 2011-2015 Marcin Kulik <http://ku1ik.com/>
#
# Distributed under the GNU General Public License, version 3.0.

set -e

interval=$(git config dude.interval || true)
interval=${interval:-60}
app_name=$(basename $0)

export LC_ALL=C # make sure git talks english

if [[ $(git config dude.notify-command) ]]; then
  notify_cmd=$(git config dude.notify-command)
elif [ $(which notify-send 2>/dev/null) ]; then
  notify_cmd='notify-send -i "$ICON_PATH" "$TITLE" "$DESCRIPTION"'
elif [ $(which terminal-notifier 2>/dev/null) ]; then
  notify_cmd='terminal-notifier -title "$TITLE" -message"$DESCRIPTION"'
elif [ $(which growlnotify 2>/dev/null) ]; then
  notify_cmd='growlnotify -n "$app_name" --image "$ICON_PATH" -m "$DESCRIPTION" "$TITLE"'
elif [ $(which kdialog 2>/dev/null) ]; then
  notify_cmd='kdialog --icon $ICON_PATH --title "$TITLE" --passivepopup "$DESCRIPTION"'
elif [ $(which notify 2>/dev/null) ]; then
  notify_cmd='notify --type information --icon "$ICON_PATH" --group "Git Commit" --title "$TITLE" "$DESCRIPTION"'
fi

function dudenotify() {
  local ICON_PATH="$1"
  local TITLE="$2"
  local DESCRIPTION="$3"

  if [ -n "$notify_cmd" ]; then
    eval $notify_cmd
  fi

  date "+%x %X"
  echo "$TITLE"
  if [ -n "$DESCRIPTION" ]; then
    echo "$DESCRIPTION"
  fi
  echo
}

[[ -d "$1" ]] && cd $1

while true; do
  for dir_name in *; do
    if [[ -d "$dir_name" && $(cd "$dir_name"; git rev-parse --git-dir 2>/dev/null) ]]; then

      if [[ $(cd "$dir_name"; git config dude.ignore) == true ]]; then
        continue
      fi

      repo_name=$(basename "$dir_name" .git)
      cd "$dir_name"

      changes=$(git fetch 2>&1 | grep -F -- '->' | sed 's/^ [+*=!-] //')

      icon_path=$(git config dude.icon || true)
      icon_path=${icon_path:-`pwd`/icon.png}
      icon_path=${icon_path// /\\ } # escape spaces before eval
      eval icon_path=$icon_path # to expand ~

      while read -r line; do
        case $line in
          *..*)
            commit_range=$(echo "$line" | awk '{ print $1 }')
            branch_name=$(echo "$line" | awk '{ print $2 }')
            commit_messages=$(git log $commit_range --pretty=format:'%s (%an)')
            dudenotify $icon_path "New commits in $repo_name/$branch_name" "$commit_messages"
            ;;
          *new\ branch*)
            branch_name=$(echo "$line" | awk '{ print $3 }')
            dudenotify $icon_path "New branch $repo_name/$branch_name" ""
            ;;
          *new\ tag*)
            tag_name=$(echo "$line" | awk '{ print $3 }')
            dudenotify $icon_path "New tag $repo_name/$tag_name" ""
            ;;
        esac
      done <<< "$changes"

      cd - &>/dev/null
    fi
  done

  sleep $interval
done
