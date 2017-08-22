INTRO:
To get a "Netflix-like" experience, please use [AUTOSETUP.SH](https://github.com/zilexa/autosetup "AUTOSETUP.SH") to install and configure the required tools. It will include this config.yml for Flexget. 

NOTE:
This Flexget config is fully based on Jonybat's config.yml. See the CHANGELOG for a full list of changes made to his config.yml. 

In non-Flexget/non-technical terms, this config will allow Flexget to do the following on a daily basis **AUTOMATICALLY**, some tasks are performed more often:

TVshows:
- Cleanup (purge) your Trakt account by removing fully watched series that have ended (or are cancelled).
- Gets the series titles you follow on Trakt.
- Finds the next episode you need based on your Watched Status in Trakt.
- Looks for these series on your own drive. 
- Looks for any manually .torrent files in your Downloads\tempmedia folder.
- Looks for the latest episodes on RSS feeds.
- Looks for your old series season packs and single episodes by discovering them on several websites.
- Downloads if they match your requirements to prevent low quality files or language specific versions from being downloaded. 
- Download regular HDTV quality to save space, this is an acceptable quality. If not found, accept 720p/1080p HD quality on the next run.
- Save to /TVDB seriesname/seasonnumber/ folders so that 1) Kodi will recognise the correct tvshow and to make it easy to cleanup your drive (simply delete the season folders). 
- Download the main file only, no other clutter that is usually present. 

Subtitles:
- Find subtitles when the download is finished.
- Add downloaded files without subs to the subtitle queue, Flexget will continue searching subs even after the files have been moved/renamed to the proper location.

Movies:
- Cleanup (purge) your Trakt account by removing movies you already have from the Trakt watchlist.
- Gets the movies you would like to see from your Trakt "watchlist".
- Looks for movies on your drive and removes them from your Trakt "watchlist". 
- Looks for any manually downloaded .torrent file in your Downloads\tempmedia folder.
- Looks for movies by discovering them on several websites. 
- Downloads them but the main file only and only if there is a match with quality requirement: no prereleases or cinema recordings. Only Bluray rips. 

Organising everything:
- Searches and downloads matching subtitles for all downloads.
- Moves & renames files to organise them properly in your folders (Movies\movie name\..., TVshows\series name\...). The series/movies title, episode or year, quality. This allows you to easily search and find subtitles in the future using your TV remote during playblack in Kodi. Note that Flexget is much better at this task so chance is very slim you ever need to do this.
- Keeps Transmission, which is used for downloading, clean. 

And last, but not least: 
- Triggers an update of your Kodi library after files have been processed! They will appear in your Kodi library automatically!
