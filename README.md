To get a "Netflix-like" experience, please use AUTOSETUP.SH to install and configure the required tools. It will include this config.yml for Flexget. 

Details:
Fully based on Jonybat's config.yml. See the CHANGELOG for all changes made to his config.yml. 

To get a "Netflix-like" experience, this config.yml does the following (in non-technical terms):
TVshows:
- Gets the series titles you follow on Trakt.
- Gets the next episode you need based on your Watched Status in Trakt.
- Looks for these series on your own drive. 
- Looks for any manually .torrent files in your Downloads\tempmedia folder.
- Looks for your series episodes on RSS feeds.
- Looks for your old series season packs(!) and single episodes by discovering them on several websites.
- Downloads if they match your requirements to prevent low quality files or language specific versions from being downloaded. 
- Makes sure to download regular HDTV quality to save space but will accept 720p/1080p quality if regular quality is unavailable.
- Cleanup your Trakt watchlist for series that have been fully watched and are no longer running. 

Movies:
- Gets the movies you would like to see from your Trakt "watchlist".
- Looks for movies on your drive and removes them from your Trakt "watchlist". 
- Looks for any manually downloaded .torrent file in your Downloads\tempmedia folder.
- Looks for movies by discovering them on several websites. 
- Downloads them but the main file only and only if there is a match with quality requirement: no prereleases or cinema recordings. Only Bluray rips. 

Organising everything:
- Searches and downloads matching subtitles for all downloads.
- Moves & renames files to organise them properly in your folders (Movies\movie name\..., TVshows\series name\...). The series/movies title, episode or year, quality and releasegroup will be in the name. This allows you to search and find subtitles using your TV remote during playblack in Kodi.
- Keeps Transmission, which is used for downloading, clean. 
- Triggers an update of your Kodi library after files have been processed.
