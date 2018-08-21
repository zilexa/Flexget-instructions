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

_Movies:_
- Gets the movies you would like to see from your Trakt "watchlist".
- Looks for movies on your drive and removes them from your Trakt "watchlist". 
- Looks for any manually downloaded .torrent file in your Downloads\tempmedia folder.
- Looks for movies by discovering them on several websites. 
- Quality: no prereleases or cinema recordings. Only Bluray rips in 1080p with a minimum filesize. But there are 2 fallbacks:
- If 1080p with a certain minimum filesize is not available, it will fallback to a lower minimal filesize.  
- If also not available, it will fallback to 720p with a minimum filesize treshold for 720p. 

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
- Cleanup (purge) your Trakt account by removing fully watched series that have ended (or are cancelled) 
- Delete old seasons from your harddrive after you have started watching the next season
- Delete tv shows from your harddrive after you have watched all seasons and the series has ended. 

- - - -

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

- - - -
Frequently Asked Questions:
>To edit your config file, `nano ~/flexget/config.yml` or use Filezilla to access your device via SFTP and edit config.yml with your favorite text editor (highly recommended).
<details><summary>How to change desired subtitle language?</summary>
<p> 
Default for series: search for 720p releases, if not found, accept the highest available quality up to 1080p. This means 720p is first preferred but if not found, 1080p will be selected, if also not found, any lower but acceptable (=HDTV release) will be accepted.
Default for movies: 3 quality buckets (HQ/NQ/LQ): 1080p 8-25GB, 1080P 2-8GB, 720P 1-8GB. Bitrate is more important that is why the buckets have filesize requirements. 
Change quality options: 
For series: search for " configure_series:". The default setting is 720p,  
For movies: find the HQ/NQ/LQ options. 
Please have a look at [this table](https://flexget.com/Plugins/quality) to understand the quality options and [this wiki](https://flexget.com/Plugins/series/timeframe) to understand how it works.
</p></details>

<details><summary>How to change desired subtitle language?</summary>
<p>
In the "template" section at the beginning of config.yml, find the "rejections" section. 
Make sure your language is not listed. By default, no translated content is accepted. Only original language content. Also Hindi is excluded. you might want to include that for Bollymovies. 
For subtitles, in the "tasks" section, find tasks "get-subtitles" and "find-subtitles". You can modify but also also add other languages. 
</p></details>

<details><summary>How to change add RSS feeds and other sources?</summary>
You can add RSS feeds to config.yml in the task "download-series-rss".
You can add search engines to the 'from' part of *-discover tasks ( download-seasons-discover,  download-series-discover and the 3 movies discover tasks). 
You might need an urlrewrite, these are part of the "torrent" config, which is located below the "templates" and above the "tasks". Ask for help on the flexget forum (https://discuss.flexget.com). 
<p>
</p></details>

<details><summary>How to upgrade Flexget?</summary>
<p>
If you have installed Flexget using Autosetup.sh OR manually by running the commands from autosetup.sh yourself, this is the only correct way to upgrade flexget:
Check your version and the latest: `~/flexget/bin/flexget -V` 
stop flexget daemon: `sudo systemctl stop flexget`
upgrade setuptools: `sudo pip3 install --upgrade setuptools`
upgrade pip3: `pip3 install --upgrade pip` #not sure if necessary but won't do harm
go to flexget folder: `cd ~/flexget/`
upgrade upgrade pip: `bin/pip install --upgrade pip`
upgrade upgrade flexget: `bin/pip install --upgrade flexget`
activate the virtualenv: `source ~/flexget/bin/activate`
upgrade transmissionrpc: `pip3 install transmissionrpc --upgrade` #loptional, last update was 2013
upgrade subliminal: `pip3 install subliminal --upgrade` #optional, last update was 2016
`exit`
optional: on it's next run, Flexget will upgrade it's database if needed. This might cause issues. You can delete your database (`rm -r ~/flexget/db-config.sqlite`) and do the 2 "first run" tasks again (authorizing Trakt and run with execute --now). 
</p></details>
