# How to Install X Tweet Fetcher Skill

This guide explains how to install and use the x-tweet-fetcher skill to read X (Twitter) tweets without API keys.

## Prerequisites

- Python 3.7 or higher
- OpenClaw installed
- clawhub CLI installed

## Installation Steps

### Step 1: Install clawhub CLI

If you haven't installed clawhub yet, run:

```bash
npm install -g clawhub
```

### Step 2: Search for the Skill

```bash
clawhub search "x tweet fetcher"
```

### Step 3: Install the Skill

```bash
clawhub install x-tweet-fetcher
```

### Step 4: Verify Installation

Check if the skill is ready:

```bash
openclaw skills check
```

You should see `x-tweet-fetcher` in the "Ready to use" list.

## Usage

### Basic Command

```bash
python <skill-path>/scripts/fetch_tweet.py --url "<tweet-url>" [--pretty] [--text-only]
```

Where:
- `<skill-path>` is typically `C:\workspace\skills\x-tweet-fetcher` on Windows or `~/.openclaw/workspace/skills/x-tweet-fetcher` on Mac/Linux

### Options

| Option | Description |
|--------|-------------|
| `--url`, `-u` | X tweet URL (required) |
| `--pretty`, `-p` | Pretty print JSON output |
| `--text-only`, `-t` | Output only the tweet text |

### Supported URL Formats

- `https://x.com/<username>/status/<tweet_id>`
- `https://x.com/i/status/<tweet_id>`
- `https://twitter.com/<username>/status/<tweet_id>`

## Examples

### Example 1: Get Tweet Text Only

```bash
python C:\workspace\skills\x-tweet-fetcher\scripts\fetch_tweet.py --url "https://x.com/jack/status/20" --text-only
```

Output:
```
@jack: just setting up my twttr

Likes: 292691 | Retweets: 123004 | Views: None
```

### Example 2: Get Full JSON

```bash
python C:\workspace\skills\x-tweet-fetcher\scripts\fetch_tweet.py --url "https://x.com/elonmusk/status/187777000000000000" --pretty
```

### Example 3: Get Long Tweet (Note Tweet)

```bash
python C:\workspace\skills\x-tweet-fetcher\scripts\fetch_tweet.py --url "https://x.com/0xValkyrie_ai/status/2024298225113674202" --text-only
```

## Output Fields

| Field | Description |
|-------|-------------|
| `text` | Tweet content |
| `author` | Author's display name |
| `screen_name` | Author's username |
| `likes` | Number of likes |
| `retweets` | Number of retweets |
| `views` | Number of views |
| `replies_count` | Number of replies |
| `created_at` | Publication timestamp |
| `is_note_tweet` | Whether it's a long tweet (Note Tweet) |
| `is_article` | Whether it's an X Article |
| `article.full_text` | Full content of X Article |
| `quote` | Quoted tweet (if any) |

## Troubleshooting

### Issue: "HTTP 404 Not Found"

- The tweet may have been deleted
- The URL format may be incorrect
- Try using the full URL format: `https://x.com/username/status/tweet_id`

### Issue: Unicode Encoding Error

If you see encoding errors on Windows, try using `--pretty` flag or redirect output to a file.

### Issue: Skill Not Found

Make sure the skill is installed:
```bash
openclaw skills check
```

## Features

- No API key required
- No login required
- Free to use
- Supports regular tweets
- Supports Note Tweets (long tweets)
- Supports X Articles
- Supports quoted tweets
- Returns engagement metrics (likes, retweets, views)

## Limitations

- Cannot fetch replies/threads (only reply count)
- Cannot fetch deleted or protected tweets
- Depends on FxTwitter API availability

## Technical Details

This skill uses the FxTwitter public API to fetch tweet data. It requires no authentication and has zero dependencies (uses Python standard library only).

For more information, visit: https://github.com/FxEmbed/FxEmbed
