#!/bin/bash
CATEGORY="$1"
PATH_="$2"
NAME="$3"
LIBRARY="/media/library/music"

[ "$CATEGORY" != "music" ] && exit 0

# Extract artist from "Artist - Album ..." format
ARTIST=$(echo "$NAME" | sed -n 's/^\(.*\) - .*/\1/p')

# Fallback if pattern doesn't match
[ -z "$ARTIST" ] && ARTIST="unsorted"

mkdir -p "$LIBRARY/$ARTIST"
cp -al "$PATH_" "$LIBRARY/$ARTIST/"
