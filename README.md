# rss_audio

A personal podcast hosting system using Jekyll, GitHub Pages, and Cloudflare R2.

**Live Feed:** https://05802.github.io/rss_audio/podcast.rss

---

## How It Works

This repo is a Jekyll site that generates a podcast RSS feed. Audio files live on Cloudflare R2 (not in this repo), and episode metadata lives here as markdown posts. GitHub Pages builds and serves the RSS feed automatically on push.

```
┌─────────────────┐      ┌─────────────────┐      ┌─────────────────┐
│   Audio Files   │      │   This Repo     │      │  Podcast Apps   │
│   (R2 Bucket)   │◄─────│   (Jekyll)      │─────►│  (iTunes, etc)  │
│                 │  URL │                 │  RSS │                 │
└─────────────────┘      └─────────────────┘      └─────────────────┘
```

**The flow:**
1. Upload an MP3 to R2 (manually or via `yt-dlp-rss`)
2. Create a markdown post in `_posts/` with the R2 URL
3. Push to GitHub
4. Jekyll builds the RSS feed
5. Podcast apps pull the feed and stream audio directly from R2

---

## Important Files

| File | Purpose |
|------|---------|
| `_posts/*.md` | Episode metadata. Each file = one episode. |
| `podcast.rss` | Jekyll template that generates the RSS feed |
| `_config.yml` | Site config (title, author, base URL) |
| `_layouts/podcast.html` | HTML page layout for individual episodes |
| `_layouts/default.html` | Base HTML template |
| `index.md` | Homepage |
| `assets/img/logo.png` | Podcast artwork (used by iTunes) |

---

## Episode Post Format

Create a file in `_posts/` named `YYYY-MM-DD-slug.md`:

```yaml
---
title: "Episode Title"
date: 2026-01-05 15:12:33 +0000
keywords:
- keyword1
- keyword2
mp3-url: "https://pub-0eced6e15a5c47f7aa40f1e1dea66c92.r2.dev/2026-01-05-slug.mp3"
explicit: "no"
block: "no"
layout: podcast
excerpt_separator: <!--more-->
---
Jan 05, 2026

Episode description goes here. This appears in podcast apps.

Timestamps, show notes, links, etc.

<!--more-->

2026-01-05-slug.md
```

**Required fields:** `title`, `date`, `mp3-url`, `layout: podcast`

---

## Adding Episodes

### Automated: yt-dlp-rss (macOS)

The `yt-dlp-rss` script automates the entire workflow for YouTube videos:

1. Prompts for a YouTube URL (pre-fills from clipboard)
2. Downloads audio via `yt-dlp`
3. Uses Gemini API to generate a descriptive filename
4. Uploads MP3 to R2 via `rclone`
5. Creates the GitHub post via API
6. Opens the post in browser for editing

**Dependencies:** `yt-dlp`, `rclone`, `jq`
**API Keys:** Stored in macOS Keychain (Gemini + GitHub token)

### Manual

1. Upload your MP3 to R2 (via dashboard or `rclone`)
2. Copy the template: `_posts/yyyy-mm-dd-short-descriptive-title[template].md`
3. Fill in the metadata and R2 URL
4. Commit and push

---

## R2 Configuration

**Bucket URL:** `https://pub-0eced6e15a5c47f7aa40f1e1dea66c92.r2.dev/`

Audio files follow the naming convention: `YYYY-MM-DD-slug.mp3`

### rclone setup

```ini
[ak_r2]
type = s3
provider = Cloudflare
access_key_id = YOUR_KEY
secret_access_key = YOUR_SECRET
endpoint = https://ACCOUNT_ID.r2.cloudflarestorage.com
```

Upload: `rclone copy file.mp3 ak_r2:rss-audio/`

---

## R2 Pricing

R2 is free for this use case.

| Resource | Free Tier | Typical Usage |
|----------|-----------|---------------|
| Storage | 10 GB/month | ~80 MB (19 episodes) |
| Downloads | 10M requests/month | Depends on listeners |
| Bandwidth | **Always free** | $0 forever |

You'd need 2,500+ episodes or 10M downloads/month to start paying.

---

## Local Development

```bash
bundle install
bundle exec jekyll serve
```

Preview at `http://localhost:4000/rss_audio/`

---

## Submitting to Podcast Platforms

Use the RSS feed URL: `https://05802.github.io/rss_audio/podcast.rss`

- [Apple Podcasts Connect](https://podcastsconnect.apple.com/)
- [Spotify for Podcasters](https://podcasters.spotify.com/)
- [Google Podcasts Manager](https://podcastsmanager.google.com/)

---

## License

See [LICENSE](LICENSE).
