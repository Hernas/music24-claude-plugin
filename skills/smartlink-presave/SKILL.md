---
name: smartlink-presave
description: Set up a Music24 SmartLink for a released song, or a pre-save page for a song that is not out yet, publish it and read its numbers. Use when the user asks for a SmartLink, a link page, a pre-save, a "link in bio" for a release, or how their SmartLink or pre-save is doing.
---

# SmartLink and pre-save

Build one SmartLink (a released song) or one pre-save page (a song that is not out yet), publish it
after a yes, and report how it does.

## Rules for every step

- **Preview, ask, confirm.** `music24_set_smartlink_published` with `published: true`,
  `music24_delete_smartlink`, a block removal in `music24_edit_smartlink_blocks`, and the 'release'
  and 'cancel' actions of `music24_update_presave` answer
  `{ needsConfirmation: true, preview, confirmToken }` first. Show the `preview`, ask the user, and
  only after a yes call the same tool again with the same arguments plus `confirmToken`.
- A new page is a draft. Nothing is public until the user approves the publish preview. A live page
  counts against the plan.
- Never invent service links. Use the links the user gives, or the links of the song from
  `music24_get_song`.

## Workflow

1. **Pick the artist and the song.** Call `music24_list_artists`, then `music24_list_catalog` and
   `music24_get_song` (title, release date, links, artwork).
2. **Check what exists.** Call `music24_list_smartlinks`. If the song already has a page, offer to
   change it instead of making a second one.
3. **Make the draft.**
   - Released song: `music24_create_smartlink` kind 'release' (`title`, `subtitle`, `urls` of the
     song on the services, `imageId` or `imageURL`).
   - Not out yet: `music24_create_smartlink` kind 'presave' (`title`, `artistName`, `releaseDate`,
     `services`, `catalogItemUuid`).
   - An existing page with no campaign: `music24_update_presave` action 'create', then add a presave
     block with `music24_edit_smartlink_blocks`.
4. **Shape the page.** Read it with `music24_get_smartlink`. Add or change blocks with
   `music24_edit_smartlink_blocks` (info, link, player, socialLinks, profile, presave, media).
   Change the name, the address (`slug`), the background or the theme with
   `music24_update_smartlink`.
5. **Publish.** Call `music24_set_smartlink_published` with `published: true`. Show the preview, ask,
   then call again with `confirmToken`. Give the user the live address.
6. **Numbers.** `music24_get_smartlink_stats` with `smartLinkId` gives the views, the clicks per
   service and the countries. `music24_get_presave` with `campaignId` gives the pre-save sign-ups.
7. **Release day of a pre-save.** When the song is out, `music24_update_presave` action 'release'
   with the song links saves the song for every fan (preview first). Then offer to turn the page
   into a release page with `music24_edit_smartlink_blocks`.
8. **Fans.** Only when the user asks, `music24_export_presave_fans` lists the fans who pre-saved.
   `music24_get_fans` gives the numbers of the fans who subscribed to every new release of the
   artist and the releases sent to them; `music24_export_fans` lists those fans. Only the workspace
   owner sees fan emails. Do not repeat emails in the chat unless the owner asks.

## Do not

- Do not publish, release a pre-save, cancel a campaign or delete a page without a yes on the
  preview.
- Do not add a tracking pixel (`music24_manage_trackers`) unless the user pastes the pixel code.
  `music24_list_trackers` shows the pixels that are already on the pages.
- Do not make AI cover pictures for the page on your own. When the song has no artwork, offer
  `music24_make_cover_pictures` (the `release-kit` skill): it uses the monthly AI picture quota and
  answers a preview first. Else use the song artwork or a kit image.
- Do not promise a plan change. When a tool says the plan does not include something, tell the
  user and point them to the Music24 app.
