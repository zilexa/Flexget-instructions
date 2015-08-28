##My [Flexget](https://github.com/Flexget/Flexget) config

This is not a comprehensive feature list, and many settings were tuned to personal preference. It is also under constant development.

It currently does the following (ideally by this order):
* Populates the series database with a set interval (maily to allow fresh starts of the series db)
* Gets series from a custom trakt list and writes them to a file, and then looks up for their tvdb name, to have series names ready for kodi (since trakt v2 all series have the year in the name)
* Looks on the series download folder for .torrent files and adds them to transmission
* Looks for series on RSS feeds and downloads them
* Discovers and downloads series that don't have any download history or are missing (with a set interval because it should not happen often)
* Does the last 2 tasks, but this time for anime
* Looks for movies in the drive and removes them from both the movie queue and the trakt list (this has a similar effect to the populate task for series db)
* Fills the movie queue with the movies in the trakt list
* Looks on the movies download folder for .torrent files and adds them to transmission
* Discovers and downloads 1080p movies and 720p movies with 1 day delay (to allow wait for a 1080p release but still be able to get movies where 1080p might not be an option), and removes them from the trakt list if downloaded
* Looks on the download folders for series and movies, and moves them to the respective folder on the drive, while renaming it and adding it to the subtitle queue
* Does a similar thing for anime, with the exception of the subtitles, but also adding the filename to a file to allow anime renaming from an external program
* Downloads subtitles for the files in the subtitle queue
* Cleans the finished torrents from transmission (delay of 1 day to allow checking transmission's webui for errors and alike)
* Updates the trakt series list from the series in the library, meaning that manually downloaded series will be part of flexget's series search, with the exception of when the tv show has already ended (with an interval of 1 day, because it should not happen often)
* Finally, it will look if all the series currently in the trakt series list are still running, or have been canceled/ended, and if so, remove them from that list (with a big interval to avoid false positives)

As sources for both tv shows and movies it uses torrentz and kat, and nyaa for anime.

It also has Pushbullet notifications for most tasks (except the ones that may spam, like move_series) and the log_filter plugin (by [tarzasai](https://github.com/tarzasai/.flexget)) to filter some log messages that are unnecessary.

This was built based on multiple configurations and snippets over the time, with the help of [flexget's community](http://discuss.flexget.com/).

Installation
------------
* Rename secrets.yml.sample to secrets.yml and change the fields inside according to your accounts and system
* Create the trakt.tv lists accordingly
* Change the paths in the move tasks to match your system
* Alternative names for series can be defined directly with the series plugin in the series template
* If you plan on using the anime rename script, you will need to install and configure [kiara](https://github.com/hartfelt/kiara/) and create the anime rename list file. If not just remove the lines marked in the move-anime task
* Remove/edit everything else that does not fit your setup and needs
* Finally, add flexget to cron (i use 30m, but i guess 1/2h is also fine): `*/30 * * * * /usr/local/bin/flexget execute`

This config was designed and tested for Linux and Transmission. It will NOT work on Windows out of the box.
