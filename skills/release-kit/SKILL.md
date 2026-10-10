---
name: release-kit
description: Make, review and download the Music24 release kit of one song - feed posts, stories, link previews, lyric and visualiser videos laid out from the artwork, lyrics and master. Use when the user asks for release assets, a release kit, promo images, a lyric video, a new version of an asset, or the kit download.
---

# Release kit

Make the kit assets of one song in the Music24 Studio, check them with the user and hand over the
files.

## Rules for every step

- **Preview, ask, confirm.** `music24_make_assets` and `music24_remake_assets` always answer
  `{ needsConfirmation: true, preview, confirmToken }` first. The preview names the assets and the
  credits (images cost 0, short videos 1, full-song videos 3). Show it, ask the user, and only after
  a yes call the same tool again with the same arguments plus `confirmToken`.
- Never invent a `themeId` or a style. Read them from `music24_list_asset_types`.
- The artist with `isDemo: true` (LaziiBee) is an example. You can read it, but you cannot change it.

## Workflow

1. **Pick the artist and the song.** Call `music24_list_artists`, then `music24_list_releases` (or
   `music24_list_catalog` for a song with no kit yet). Call `music24_get_song`: a video needs a ready
   master, and a lyric video needs lyrics. If the master is missing, ask for a public https link and
   call `music24_import_file` with `target: 'master'`, then `music24_get_import` until it is done.
   Send `rightsConfirmed: true` only when the user says they own or control the rights.
2. **Read the kit.** Call `music24_get_release_kit` with the song. Say which assets are ready,
   which are working and which failed.
3. **Look and theme.** If the user wants another look, call `music24_set_release_theme` (theme,
   font, five colours, logo). `resetDefault: true` answers a preview first.
4. **Pick the assets.** Call `music24_list_asset_types`. Suggest a short set: images first (no
   credits), then short videos. A full-song video needs Pro (`needsFullVideoPlan`).
5. **Make them.** Call `music24_make_assets` with 1 to 20 items. Show the preview with the credits,
   ask, then call again with `confirmToken`. The answer lists `created`, `busy` and `skipped`.
6. **Follow up.** Call `music24_get_release_kit` with `work: true` to see what is still being made.
   When an asset is ready, `music24_list_asset_versions` gives its file links.
7. **Make again.** If the user does not like an asset, call `music24_remake_assets` (preview first).
8. **Hand over.** For one file, `music24_share_file` makes a public share link (preview first:
   anyone with the link can see the file). For the whole kit, `music24_get_kit_zip`. For a motion
   artwork that a distributor needs as ProRes, `music24_make_prores` starts the file and
   `music24_get_prores` reads it when it is ready (Pro and higher).
9. **Next.** Offer the `release-rollout` skill to put the assets into posts.

## Do not

- Do not ask for AI footage, AI reels, AI cover pictures or AI clips. This Claude connector does not
  make AI images, video or audio. Asset types and styles that need AI footage are not offered here;
  a request for one answers `M24_MCP_NOT_IN_HOST`.
- Do not make a music video from a cut that plays clip-bank or AI clips. That also answers
  `M24_MCP_NOT_IN_HOST`.
- Do not spend credits without a yes on the preview.
- Do not share a file publicly unless the user asks for a share link.
