# Music24 for Claude

Run your music release from Claude. This plugin connects Claude to your Music24 account and adds
five workflows that artists and their teams use every week:

| Skill | Ask for example |
|---|---|
| `release-rollout` | "Plan the rollout of my new single and schedule the first week of posts." |
| `weekly-check-in` | "What moved this week for my artist?" |
| `release-kit` | "Make the release kit images for my new song and show me what is ready." |
| `smartlink-presave` | "Make a pre-save page for my next song and publish it." |
| `chart-watch` | "Who is rising on the charts today? Follow the top three." |

Claude reads your catalog, release kits, rollout plans, posts, SmartLinks, pre-saves, streaming and
social numbers, and the public Music24 charts. It can make kit assets, plan and schedule posts and
build SmartLinks for you. Before anything goes public, uses credits or deletes something, Music24
answers with a preview. Claude shows it to you and waits for your yes.

## Connect

1. Install the plugin. It adds the Music24 MCP server `https://api.music24.com/mcp/claude`.
2. Sign in at [music24.com/app](https://music24.com/app) in your browser and pick the workspace
   that Claude should work in.
3. In Claude, connect Music24. A Music24 page opens: select **Approve**.

You need a Music24 account. Every plan works, Free included. Your plan limits and credits apply as
in the Music24 app.

## Access (OAuth scopes)

- `music24:read`: see your artists, releases, numbers and posts.
- `music24:write`: make assets, plan and schedule posts, and edit SmartLinks for you.

Claude never gets your password or a personal token. To disconnect, open **Settings → API →
Connected apps** in the Music24 app and select **Revoke**.

## What this plugin does not do

This Claude listing does not make AI images, AI video or AI audio: no AI reels, no AI cover
pictures and no AI clips. Kit images and videos that Music24 lays out from your artwork, lyrics and
master stay available.

## Privacy and support

- Privacy policy: https://music24.com/privacy-policy
- Terms of service: https://music24.com/terms-of-service
- Help: https://music24.com/help/api-and-mcp/mcp-and-oauth
- Support: support@music24.com

## License

MIT. See [LICENSE](LICENSE).
