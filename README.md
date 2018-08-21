Want to have free choice of on-demand tv content? Are on-demand subscription services limiting the available content and forcing you to watch a series within a few months? Did your favourite tvshow or movie became unavailable or is your TV subscription only offering you low quality crap? 
Enjoy the freedom. Keep paying for all those, since it is the only way to pay for content (due too a stubborn industry) but also go for free choice.  

The way it works: on http://trakt.tv you can create a free account, add TV Shows titles you like to follow to a list (call that list tvshows). Trakt already has a list for movies, it is called "watchlist". Add the Trakt.tv addon to your Kodi mediacenter device. Kodi will sync your watched status with Trakt.tv (privately). 
Trakt does not offer any media content. It is purely a wikipedia for tv and movies. You can use Trakt.tv or just the internet in general to discover the shows and movies you would like to add to your tvshows list and your movies watchlist. By using trakt we now have an overview of 1) what you want to watch and 2) which episodes or movies you do not need, since you have already watched them.

FLEXGET
Flexget is an amazingly powerful yet lightweight tool. There is no UI, it will simply run as service in the background. It is fully open source and developed and maintained by volunteers. you can support them here: https://flexget.com/ by donating.
It uses your Trakt list and watched status to find, select, download,  organise, enrich (with subtitles) and cleanup the content you want and organise everything in such a way to deliver the Lazy Couch Experience!

Content is downloaded using RSS Feeds and search engines that provide magnet links to torrent hives. 

### What does it do exactly? ###
In non-Flexget/non-technical terms, this guide will allow Flexget to leverage the CONFIG.YML in this repository to do the following on a daily basis **AUTOMATICALLY**, some tasks are performed more often and for efficiency, not necessarily in this order:

<details><summary>What Flexget does for TVshows...</summary>
<p><ul>
<li>Get the series titles you follow on Trakt.</li>
<li>Finds the next episode you need based on your Watched Status in Trakt.</li>
<li>Checks which series episodes you might already have on your drive.</li>
<li>Looks for the latest episodes on RSS feeds.</li>
<li>Looks for your old series seasons and single episodes by discovering them on several websites.</li>
<li>Downloads if they match your requirements to prevent low quality files or language specific versions from being downloaded. </li>
<li>Download 720p HD quality, if not found, accept 1080p HD quality on the second run.</li>
</ul></p></details>

<details><summary>What Flexget does for Movies...</summary>
<p><ul>
<li> Gets the movies you would like to see from your Trakt "watchlist".</li>
<li> Looks for movies on your drive and removes them from your Trakt "watchlist". </li>
<li> Looks for any manually downloaded .torrent file in your Downloads\tempmedia folder.</li>
<li> Looks for movies by discovering them on several websites. </li>
<li> Quality: no prereleases or cinema recordings. Only Bluray rips in 1080p with a minimum filesize. But there are 2 fallbacks:</li>
<li> If 1080p with a certain minimum filesize is not available, it will fallback to a lower minimal filesize.  </li>
<li> If also not available, it will fallback to 720p with a minimum filesize treshold for 720p. </li>
</ul></p></details>

<details><summary>And even subtitles...</summary>
<p><ul>
<li> Find subtitles when the download is finished using the original filename. </li>
<li> Add downloaded files without subs to the subtitle queue. </li>
<li> Keep searching for subtitles for all files in the queue even after they have been renamed and moved to their proper location.</li>
</ul></p></details>

<details><summary>How flexget organises everything..</summary>
<p><ul>
<li> Purges Transmission, which is used for downloading. This will cleanup Transmission.</li>
<li> Always downloads the main file only, no other files that are usually present. No more clutter! </li>
<li> Moves & renames tv shows and movies after download and subtitle search is done and organises them properly. </li>
<li> All episodes are saved in season<li>specific folders. This allows you to simply delete a season folder to free up space.  </li>
<li> Filenames will contain series or movies title, episode or year and quality release. </li>
</ul></p></details>

<details><summary>And last, but not least</summary>
<p><ul>
<li> Triggers an update of your Kodi library after files have been processed! They will appear in your Kodi library automatically. </li>
<li> Cleanup (purge) your Trakt account by removing fully watched series that have ended (or are cancelled) </li>
<li> Delete old seasons from your harddrive after you have started watching the next season</li>
<li> Delete tv shows from your harddrive after you have watched all seasons and the series has ended. </li>
</ul></p></details>

- - - -

### Installation of software ###
Hardware: a Raspberry Pi 3 or (recommended) [Vero 4K](https://osmc.tv/store/). You can use any Debian based OS but I highly recommend [OSMC, "Open Source Meda Center"](https://osmc.tv/download/). It is an OS for Raspberry Pi and Vero, based on Debian and perfectly tweaked to run Kodi on those devices. Also buy a 2TB external USB harddrive. I highly recommend to go for a silent one, NOT a fast one (2.5", 7mm, 5400rpm is enough). 

1. After commecting your RPi3 or Vero to your tv, installing or updating OSMC (follow the instructions on https://osmc.tv/download) use your TV remote and go to MyOSMC > App Store and install Transmission (recommended: also go to Services and enable SAMBA server to be able to view your files on your Windows or Android device). 

2. Create a (free, private) account on https://Trakt.tv. Create a list called "tvshows" and add the shows you would like to see, make sure these shows are not "ended" on trakt. Also mark episodes/seasons you have already seen as watched otherwise they will be downloaded. 

3. If you need subtitles, create accounts with the same user/pw on opensubtitles.org and addic7ed.com.

4. Create an account at showrss.info, add the series you like and go to your personal feed. write down the userID number you see in the url, for example http://showrss.info/user/1234.rss?...., the userid is 1234. 

5. Use [AUTOSETUP.SH](https://github.com/zilexa/autosetup "AUTOSETUP.SH") to install. All you have to do is add your Trakt account, showrss account, subtitles account, Transmission login and the location of your harddrive. Then you can run the file and sit back while it installs. More information via the link. 
- Note you need to add all of this information manually to secrets.yml and config.yml (showrss user id) if you do not use Autosetup!. 

#### 6. Configure Transmission to run 2 Flexget tasks after each completed download ####
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

#### 7. FIRST RUN: authorize flexget to use Trakt and run flexget once ####
- Authorize Flexget to access Trakt (you need a phone or pc and login to Trakt website to finish this step): 
`~/flexget/bin/flexget trakt auth YOURTRAKTUSERNAME`

- Run Flexget once fully: 
`~/flexget/bin/flexget execute --now`

#### 8. START THE SERVICE and go have your lazy couch experience ####
- Start the service, it has been enabled by Autosetup already, only need to start it (will also happen on reboot): 
`sudo systemctl start flexget`

- - - -
### Frequently Asked Questions ###
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
<p>You can add RSS feeds to config.yml in the task "download-series-rss".
You can add search engines to the 'from' part of *-discover tasks ( download-seasons-discover,  download-series-discover and the 3 movies discover tasks). 
You might need an urlrewrite, these are part of the "torrent" config, which is located below the "templates" and above the "tasks". Ask for help on the flexget forum (https://discuss.flexget.com). 

</p></details>

<details><summary>How to upgrade Flexget?</summary>
<p>
If you have installed Flexget using Autosetup.sh OR manually by running the commands from autosetup.sh yourself, this is the only correct way to upgrade flexget:
  <ul><li>Check your version and the latest: <code>~/flexget/bin/flexget -V</code> </li>
<li>stop the flexget service: <code>sudo systemctl stop flexget</code></li>
<li>upgrade setuptools: <code>sudo pip3 install --upgrade setuptools</code></li>
<li>upgrade pip3: <code>pip3 install --upgrade pip</code> #not sure if necessary but won't do harm</li>
<li>go to flexget folder: <code>cd ~/flexget/</code></li>
<li>upgrade upgrade pip: <code>bin/pip install --upgrade pip</code></li>
<li>upgrade upgrade flexget: <code>bin/pip install --upgrade flexget</code></li>
<li>activate the virtualenv: <code>source ~/flexget/bin/activate</code></li>
<li>upgrade transmissionrpc: <code>pip3 install transmissionrpc --upgrade</code> #loptional, last update was 2013</li>
<li>upgrade subliminal: <code>pip3 install subliminal --upgrade</code> #optional, last update was 2016</li>
<li><code>exit</code></li>
<li>optional: on it's next run, Flexget will upgrade it's database if needed. This might cause issues. You can delete your database (<code>rm -r ~/flexget/db-config.sqlite</code>) and do the 2 "first run" tasks again (authorizing Trakt and run with execute --now). </li>
<li>Start the flexget service:<code>sudo systemctl start flexget</code></li>
  </ul></p></details>
