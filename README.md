# YouTube Comments Scraper

![YouTube Comments Scraper](https://raw.githubusercontent.com/smoldesk/apify-scrapers/main/assets/youtube-comments/banner.webp)

Scrape YouTube comments and replies from video URLs, a channel's latest videos, playlists or a keyword search. No API key, no quota, no login. Runs on [Apify](https://apify.com/smoldesk/youtube-comments-scraper).

## What you get

- **Comments and replies**: full text, author name and channel, likes, reply count, pinned, hearted and by-the-creator flags, approximate date.
- **Many sources in one run**: video or Shorts URLs, `@handles` (comments on the latest videos), playlists, and search terms (comments on the top videos for a keyword).
- **Top or newest first**, with a per-video cap and replies per comment.
- **`sourceInput` on every comment**, so one run can watch many brands or channels.
- **Pay only for comments**: opening the videos is free.

## Run it

Open the [YouTube Comments Scraper on Apify](https://apify.com/smoldesk/youtube-comments-scraper), fill in the form and click Start. Or call it from code with your [Apify token](https://console.apify.com/settings/integrations):

```bash
curl -X POST "https://api.apify.com/v2/acts/smoldesk~youtube-comments-scraper/run-sync-get-dataset-items?token=YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "startUrls": ["https://www.youtube.com/watch?v=dQw4w9WgXcQ", "@mkbhd"],
    "searchQueries": ["standing desk review"],
    "maxResults": 3,
    "maxComments": 200,
    "commentsSortBy": "newest",
    "maxRepliesPerComment": 5
  }'
```

Python:

```python
from apify_client import ApifyClient

client = ApifyClient("YOUR_TOKEN")
run = client.actor("smoldesk/youtube-comments-scraper").call(
    run_input={"searchQueries": ["iphone 17 review"], "maxResults": 5, "maxComments": 100}
)
for c in client.dataset(run["defaultDatasetId"]).iterate_items():
    print(c["likeCount"], c["text"][:80])
```

## Sample output

```json
{
  "type": "comment",
  "videoId": "dQw4w9WgXcQ",
  "videoTitle": "Rick Astley - Never Gonna Give You Up (Official Video) (4K Remaster)",
  "videoChannelName": "Rick Astley",
  "url": "https://www.youtube.com/watch?v=dQw4w9WgXcQ&lc=UgzKYksqeQ5gA3t0g0F4AaABAg",
  "text": "I Rick rolled my entire French class because…",
  "authorName": "@grustache4569",
  "authorChannelUrl": "https://www.youtube.com/@grustache4569",
  "likeCount": 106000,
  "replyCount": 665,
  "publishedAtApprox": "2020-11-04T03:16:17Z",
  "isPinned": false,
  "isHearted": false,
  "isReply": false,
  "parentId": null,
  "sourceInput": "https://www.youtube.com/watch?v=dQw4w9WgXcQ"
}
```

## Pricing

Pay per comment or reply. No monthly fee. The price drops on bigger Apify plans.

| Apify plan | Per 1,000 comments |
|---|---|
| Free | $1.20 |
| Starter | $1.00 |
| Scale | $0.80 |
| Business and up | $0.60 |

## Limits

- Comment dates are approximate: YouTube shows only "3 weeks ago" on public pages.
- Like and reply counts are rounded the way YouTube displays them.
- No live chat, and nothing from videos with comments turned off.

## FAQ

**Do I need a YouTube API key?**
No. There is no daily quota and no Google Cloud project to set up.

**How do I get the comments on a channel's new videos every day?**
Put the channel in `startUrls`, set `publishedAfter` to `1 day` and schedule the run daily.

**How do I get comments about a brand?**
Add the brand name to `searchQueries`. The Actor takes the top `maxResults` videos for it and saves their comments.

## More scrapers

YouTube Comments is one of 18 no-login scrapers by Smoldesk Studios. For videos and channel profiles see [YouTube Scraper](https://github.com/smoldesk/youtube-scraper) and [YouTube Channel Scraper](https://github.com/smoldesk/youtube-channel-scraper). Full list: [smoldesk/apify-scrapers](https://github.com/smoldesk/apify-scrapers).
