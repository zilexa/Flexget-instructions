This config is fully based on jonybat's flexget config. Below are all the changes I have made to his config to do everything I believe is necessary for the ultimate Netflix-like experience.


BUG:
- Next_trakt_episodes sets the starting episode correct for most series except some like Scandal (US), House of Cards (US), The Librarians (2014), Mistresses (US). This cannot be fixed easily, since "strip_dates" is required to support all other series. Spend 3 days couldn't solve it.

Removed: 
- everything anime related

Disabled: 
- Pushbullet (commented out)

Added: 
- task sync-trakt-nextep now your Trakt watched status will be used to determine the first episode to search for.

Modified: 
- "Schedules" changed to run less often. Most importantly searching RSS feeds only happens between 2AM-8AM and searching websites only once at 2AM. The new 'sync-trakt-nextep' task runs once a week. Could be changed to once (don't know how) or once a month. 
- Go for normal webrip/HDTV releases. If not available (wait 1 hr), allow Flexget to accept HD quality releases 720P or 1080P.
Reason: normal releases are good enough quality these days, 720P and 1080P eat up a lot of space. 
- template "series" added support for season_packs to be downloaded when you need whole seasons or have less than 4 episodes of a season. This also required required trakt_lookup to be added. 
- template "movies-720p" and "movies-1080p" changed the content_size. 
- task "download-series-discover" will now accept series starting the first episode set my task sync-trakt-nextep.
- task "download-series-discover" added extra sources.
- task "move-series" and task "move-movies" added 'release group' to the filename of episodes in case you ever need to manually search subtitles.
- task "move-series" and task "movie-movies" added 'kodi_library' plugin to update Kodi after a file has been moved.
- task "download-series-rss" added and updated RSS feeds. 
- task "download-series-discover" added and updated sources. 
- task "download-movies-720p" and "download-movies-1080p"  added sources.
