The way it works: on http://trakt.tv you can create a free account, add TV Shows titles you like to follow to a list (call that list tvshows) and add the Trakt.tv addon to your Kodi mediacenter device. Kodi will sync your watched status with Trakt.tv (privately). F

Flexget is an amazingly powerful yet lightweight tool. There is no UI, it will simply run as service in the background. 
It uses your Trakt list and watched status to find, select, download (RSS Feeds and magnet torrent sites) organise, enrich (with subtitles) and cleanup the content you want and organise everything in such a way to deliver the Lazy Couch Experience!

In non-Flexget/non-technical terms, this config will allow Flexget to do the following on a daily basis **AUTOMATICALLY**, some tasks are performed more often and for efficiency, not necessarily in this order:

_TVshows:_
- Gets the series titles you follow on Trakt.
- Finds the next episode you need based on your Watched Status in Trakt.
- Looks for these series on your own drive. 
- Looks for any manually .torrent files in your Downloads\tempmedia folder.
- Looks for the latest episodes on RSS feeds.
- Looks for your old series season packs and single episodes by discovering them on several websites.
- Downloads if they match your requirements to prevent low quality files or language specific versions from being downloaded. 
- Download 720p HD quality, if not found, accept 1080p HD quality on the second run.
- Cleanup (purge) your Trakt account by removing fully watched series that have ended (or are cancelled) 
- Delete whole seasons from your harddrive after you have started watching the next season or you have watched all seasons and the series has ended. 

_Movies:_
- Gets the movies you would like to see from your Trakt "watchlist".
- Looks for movies on your drive and removes them from your Trakt "watchlist". 
- Looks for any manually downloaded .torrent file in your Downloads\tempmedia folder.
- Looks for movies by discovering them on several websites. 
- Quality: no prereleases or cinema recordings. Only Bluray rips in 1080p with a minimum filesize. But there are 2 fallbacks:
- If 1080p with a certain minimum filesize is not available, it will fallback to a lower minimal filesize.  
- If also not available, it will fallback to 720p with a minimum filesize treshold for 720p. 
- Cleanup (purge) your Trakt account by removing movies you already have from the Trakt watchlist.

_Subtitles:_
- Find subtitles when the download is finished using the original filename. 
- Add downloaded files without subs to the subtitle queue. 
- Keep searching for subtitles for all files in the queue even after they have been renamed and moved to their proper location.

_Organising everything:_
- Purges Transmission, which is used for downloading. This will cleanup Transmission.
- Always downloads the main file only, no other files that are usually present. No more clutter! 
- Moves & renames tv shows and movies after download and subtitle search is done and organises them properly. 
- All episodes are saved in season-specific folders. This allows you to simply delete a season folder to free up space.  
- Filenames will contain series or movies title, episode or year and quality release. 

_And last, but not least:_
- Triggers an update of your Kodi library after files have been processed! They will appear in your Kodi library automatically. 


**Installation of software**
1. The first basic requirement: an RPi2 or RPi3 or (recommended) Vero 4K. Go to https://osmc.tv and order one. Install and OSMC. With your TV remote go to MyOSMC, App Store and install Transmission (recommended: also go to Services and enable SAMBA server). 

2. A (free, private) account on Trakt.tv. Create a list called "tvshows" and add the shows you would like to see. 
Also mark episodes/seasons you have already seen as watched otherwise they will be downloaded. 

3. If you need subtitles, create accounts with the same user/pw on opensubtitles.org and addic7ed.com.

4. Create an account at showrss.info, add the series you like and go to your feed. write down the user ID you see in the url.

5. Use [AUTOSETUP.SH](https://github.com/zilexa/autosetup "AUTOSETUP.SH") to install. All you have to do is add your Trakt account, showrss account, subtitles account, Transmission login and the location of your harddrive. Then you can run the file and sit back while it installs. More information via the link. 
- Note you need to add all of this information manually to secrets.yml and config.yml (showrss user id) if you do not use Autosetup!. 

**6. Configure Transmission to run 2 Flexget tasks after each completed download**
- `sudo systemctl stop transmission-damon`
- `cd /home/osmc/.config/transmission-deamon`
- `nano runflexget.sh`
- paste this: /home/osmc/flexget/bin/flexget execute --tasks find-* move-*  
and hit CTRL+O and CTRL+X
- `chmod +x runflexget.sh`
- `nano settings.json`
- Near the bottom, find and change accordingly: 
"script-torrent-done-enabled": true,
"script-torrent-done-filename": "/home/osmc/.config/transmission-daemon/runflexget.sh",
hit CTRL+O and CTRL+X
- `sudo systemctl stop transmission-damon`

**7. FIRST RUN: authorize flexget to use Trakt and run flexget once**
When finished, make sure you authorise trakt and run Flexget once via these two commands:
- Authorize Flexget to access Trakt (you need a phone or pc and login to Trakt website to finish this step): 
`~/flexget/bin/flexget trakt auth YOURTRAKTUSERNAME`

- Run Flexget once fully: 
`~/flexget/bin/flexget execute --now`

**8. START THE SERVICE AND LEAN BACK**
- Start the service, it has been enabled by Autosetup already, only need to start it (will also happen on reboot): 
`sudo systemctl start flexget`

`` 
`` 
OPTIONAL: change video quality and subtitle language 
- default for series: 720p, if not found, accept the highest quality up to 1080p (usually this means if 720p is not found, 1080p will be selected if available otherwise standard HDTV). 
- default for movies: 3 quality buckets (HQ/NQ/LQ): 4K-10bit-hdr, 1080P, 720P or lower. But bitrate is just as important that is why the buckets have filesize requirements. 

- Change quality options: 
`nano ~/flexget/config.yml` or use Filezilla to edit the file on your Mac/Windows Notepad. 
For series: search for " configure_series:". The default setting is 720p,  
For movies: find the HQ/NQ/LQ options. 

Please have a look at [this table](https://flexget.com/Plugins/quality) to understand the quality options and [this wiki](https://flexget.com/Plugins/series/timeframe) to understand how it works.

- Change Language:
Have a look at "rejections". Make sure your language is not listed. By default, no translated content is accepted. Only original language content. Also Hindi is excluded. you might want to include that for Bollymovies. 
For subtitles, search for "get-subtitles" and "find-subtitles". You can modify but also also add other languages. 

