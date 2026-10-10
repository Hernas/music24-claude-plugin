---
name: chart-watch
description: Read the public Music24 charts (top and rising artists and tracks, playlists, trends, markets) and manage the workspace Watchlist of follows. Use when the user asks who is rising, what is trending, how an artist or track does on playlists, to follow or unfollow an artist, track or playlist, or for the daily or weekly report of their follows.
---

# Chart watch

Read Music24's public chart data and keep the Watchlist of the workspace up to date.

## Rules for every step

- Most tools here read. `music24_follow` and `music24_unfollow` change the Watchlist that every
  member of the workspace shares: say what you will follow or unfollow before you call them.
- **Preview, ask, confirm.** `music24_share_view` answers
  `{ needsConfirmation: true, preview, confirmToken }` first (it makes a public link). Show the
  `preview`, ask the user, and only after a yes call the same tool again with the same arguments
  plus `confirmToken`.
- `music24_artist_details` and `music24_track_details` use one browse credit each. A search with
  `music24_browse_search` uses none. Check the credits left with `music24_browse_search` view
  'usage' before many detail calls.
- Use the ids from the chart or search answers. Never guess an id.

## Workflow

1. **What is on top.** `music24_top_artists` and `music24_top_tracks` with 'overall' (visibility)
   or 'gaining' (the biggest playlist-add movers on `date`). `music24_get_trends` view 'trendingUp'
   gives the biggest 7-day risers, view 'anomalies' the spikes and drops.
2. **Find something.** `music24_browse_search` with `q` finds artists, tracks and playlists.
3. **Look closer.** For an artist: `music24_artist_details` and `music24_get_artist_charts`
   ('positionChange', 'playlistScore', 'reach', 'listeningTrend'). For a track:
   `music24_track_details` and `music24_get_track_charts` (also 'topPlaylists' and
   'recentChanges'). For a playlist: `music24_get_playlist` ('details', 'songs', 'followers') and
   `music24_playlist_changes` for its recent adds and removes.
4. **Follow.** After the user agrees, `music24_follow` with `kind` and `id`. For a playlist that
   Music24 does not track yet, send its Spotify, Apple Music, Deezer or YouTube `url`.
   `music24_unfollow` removes a follow.
5. **Reports.** `music24_my_follows` lists the Watchlist. `music24_daily_report` compares a day with
   the day before; `music24_weekly_report` compares 7 days with the 7 before. Each gives the top 5
   movers. `music24_get_markets` shows the countries where the followed artists and tracks are seen.
6. **Share.** Only when the user asks, `music24_share_view` makes a public link to one chart page
   (preview first). `music24_export_watchlist` gives the Watchlist as CSV (Pro and higher).

## Do not

- Do not follow or unfollow without the user's yes: every member of the workspace sees the change.
- Do not spend many browse credits on details the user did not ask for.
- Do not present chart positions as streams. Music24 charts measure playlist visibility and adds.
- Do not make AI images, video or audio of the charts. This Claude connector does not make them.
