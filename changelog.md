This config is fully based on jonybat's flexget config. Below are all the changes I have made to his config to do everything I believe is necessary for the ultimate Netflix-like experience.

#################
##    TODO     ##
#################
The config works perfectly after the latest update of 20171109. However I do want to do 2 more things:
- Optimize the part that searches for Season packs.
- Remove log_filter.py if that plugin is no longer supported by the latest version of Flexget (2.10.106, November 2017).

#################
##  CHANGELOG  ##
#################
20171109
ALL issues have now been solved. Most of them were minor. The biggest changes:
-Solved issues with failing subtitle lookup. 
- Some episodes don't have correct SxxEyy naming (like S11E03) but look like this: 1103 or even worse: 903 (meaning S09E03).
The config has been adjusted to solve this issue.
- Movies will now first search for 8-25GB 1080p Bluray rips, if not found, 2GB-7.999GB 1080p Bluray rip, if not found, 0.9GB-8GB 720P Bluray rip. 



###################################################
##  CHANGES TO THE ORIGINAL CONFIG FROM JONYBAT  ##
###################################################
FYI this is not a complete overview.

added:
- fully based on tvdb naming scheme so that Kodi will always recognise everything.
- added task to cleanup Trakt tvshows list first. If you have fully watched a tv show AND no new seasons will follow (show has ended), it will be removed from your trakt list. This seemed like an obvious thing to add since the Movies task also cleans up your movies list. 

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
