# redyt.sh

originally from @

# redyt

### Scrape Reddit images from the terminal

At first invocation `redyt` checks if these programs are installed:

  - jq (JSON parsing)
  - dmenu (User Input)
  - sxiv (Image Viewing)

and then downloads 10 images from the subreddit `linuxmemes` ...

... from the newly created `~/.config/redyt/subreddits.lst`.

The wished subreddit(s) can be changed

a) in `redyt`'s variable `defaultsub` or
b) w/ an argument: `./redyt [subreddit-name]` or
c) from `dmenu` w/ a name of another subreddit typed in.


Here's an example of a custom `subreddits.lst`:

```
art           | Cool Artwork 🖌️ by Cool redditors
memes         | Memes! Memes! Memes! 😂 I 💖 Memes 
woahdude      | The best links to click while you're stoned! 
unixporn      | Screenshots of cool Linux rice. 🐧
dankmemes     | Memes which are hard to understand.
wallpapers    | Good wallpapers for my Desktop.
PrettyGirls   | Cool pictures of beautiful girls.
AbandonedPorn | High quality images of abandoned things and places.
```

Note: If you don't have `notify-send` installed, coreutil `echo` will be used as a notifier.
`notify-send` is also recommended, but, if not present, `echo` will be used as a notifier.
