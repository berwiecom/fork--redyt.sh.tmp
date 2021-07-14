#!/bin/sh

UserAgent='Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:56.0) Gecko/20100101 Firefox/56.0'

# TODO

# Put vars to top
# echo \t works. But w/ notify-send??
# curl.ori is used atm!

# Check if necessary programs are installed
for Prog in dmenu jq sxiv; do
 [ ! "$(which "${Prog}")" ] && echo -e "\tBitte installiere zuerst ${Prog}! ZB mit 'sudo apt install ${Prog}'" && exit 1
done

# If notify-send is not installed, use echo as notifier
[ ! "$(which notify-send)" ] && Notifier="echo" || Notifier="notify-send"

# Default config directory
ConfigDir="${XDG_CONFIG_HOME:-$HOME/.config}/redyt"

# Create .config/redyt if it does not exist to prevent
# the program from not functioning properly
[ ! -d "${ConfigDir}" ] && echo -e "\tOrdner ${ConfigDir} existiert nicht. Wird nun erzeugt. Bitte warten ..." &&
 mkdir -p "${ConfigDir}" 2>/dev/null

# Default subreddit
DefaultSub="linuxmemes"

# If subbreddit.txt does not exist, create it to prevent
# the program from not functioning properly
[ ! -f "${ConfigDir}/subreddits.lst" ] && echo -e "${DefaultSub}" >> "${ConfigDir}/subreddits.lst"

# If no argument is passed
if [ -z "$1" ]; then
	# Ask the user to enter a subreddit
	SubReddit=$(dmenu -p "\tWaehle Dein Subreddit r/" -i -l 10 < "${ConfigDir}/subreddits.lst" | cut -d\| -f1 | awk '{$1=$1;print}')

	# If no subreddit was chosen, exit
	[ -z "${SubReddit}" ] && exit 1

# Otherwise assign the first argument to the
# subreddit variable
else
	SubReddit="$1"
fi

# Default directory used to store the feed file and fetched images
CacheDir="/tmp/redyt"

# If CacheDir does not exist, create it
if [ ! -d "${CacheDir}" ]; then
	echo -e "\t${CacheDir} existiert nicht. Wird nun erzeugt."
	mkdir -p "${CacheDir}" 2>/dev/null
fi

# Limit the maximum number of requests to make
Limit=1 ########################################### TEST

# Send a notification
${Notifier} "\tRedyt" "🖼️ Memes werden 📩 heruntergeladen."

# Download the subreddit feed into $CacheDir/tmp.json
# containing only the first $Limit entries
echo curl.ori -H "User-agent: '${UserAgent}'" "https://www.reddit.com/r/${SubReddit}/hot.json?limit=${Limit}" > "${CacheDir}/tmp.json"

# Create a list of images
ImageList=$(jq '.' < "${CacheDir}/tmp.json" | grep url_overridden_by_dest | grep -Eo "http(s|)://.*(jpg|png)\b" | sort -u)

# If there are no images, exit
[ -z "${ImageList}" ] && ${Notifier} "\tRedyt:" "Leider sind keine Bilder in SubReddit '${SubReddit}' vorhanden. Bitte versuche es spaeter wieder!" && exit 1

# Download images to ${CacheDir}
wget -P "${CacheDir}" ${ImageList}

# Send a notification
${Notifier} "\tRedyt" "👍 Alle SubReddit-Bilder heruntergeladen! 😊"

# Display the images
sxiv -a "${CacheDir}"/*.png "${CacheDir}"/*.jpg

# Once finished, remove all of the cached images
rm "${CacheDir:?}"/*
