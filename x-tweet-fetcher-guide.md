# X Tweet Fetcher Skill

## 概述

这是一个用于从 X (Twitter) 抓取推文的 skill，不需要 API key，不需要登录，完全免费。

## 安装状态

- **状态**: ✅ 已安装并可用
- **路径**: `C:\workspace\skills\x-tweet-fetcher`
- **依赖**: Python 3.7+

## 使用方法

### 基本命令

```bash
python C:\workspace\skills\x-tweet-fetcher\scripts\fetch_tweet.py --url "<推文链接>" [--pretty] [--text-only]
```

### 参数说明

| 参数 | 说明 |
|------|------|
| `--url`, `-u` | X 推文链接 (必需) |
| `--pretty`, `-p` | 格式化 JSON 输出 |
| `--text-only`, `-t` | 仅输出文本内容 |

### 输出字段

| 字段 | 说明 |
|------|------|
| `text` | 推文正文 |
| `author` | 作者显示名称 |
| `screen_name` | 作者用户名 |
| `likes` | 点赞数 |
| `retweets` | 转发数 |
| `views` | 浏览量 |
| `replies_count` | 回复数 |
| `created_at` | 发布时间 |
| `is_note_tweet` | 是否为长推文 |
| `is_article` | 是否为 X 文章 |
| `article.full_text` | X 文章完整内容 |
| `quote` | 引用的推文 |

## 示例

### 示例 1: 获取单条推文

```bash
python C:\workspace\skills\x-tweet-fetcher\scripts\fetch_tweet.py --url "https://x.com/jack/status/20" --text-only
```

输出:
```
@jack: just setting up my twttr

Likes: 292691 | Retweets: 123004 | Views: None
```

### 示例 2: 获取 JSON 格式

```bash
python C:\workspace\skills\x-tweet-fetcher\scripts\fetch_tweet.py --url "https://x.com/elonmusk/status/187777000000000000" --pretty
```

### 示例 3: 获取长推文 (Note Tweet)

```bash
python C:\workspace\skills\x-tweet-fetcher\scripts\fetch_tweet.py --url "https://x.com/0xValkyrie_ai/status/2024298225113674202" --text-only
```

### 示例 4: 获取 X Article (长文章)

```bash
python C:\workspace\skills\x-tweet-fetcher\scripts\fetch_tweet.py --url "https://x.com/username/status/<article_id>" --text-only
```

## 支持的 URL 格式

- `https://x.com/<username>/status/<tweet_id>`
- `https://x.com/i/status/<tweet_id>`
- `https://twitter.com/<username>/status/<tweet_id>`
- 直接使用 tweet ID 也可以

## 注意事项

1. **链接格式**: 推文链接可以是 `x.com/username/status/id` 或 `x.com/i/status/id` 格式
2. **长推文**: 支持 Twitter Blue 长推文 (Note Tweet)
3. **X Article**: 支持 X 长文章，输出包含完整文章内容
4. **引用推文**: 如果推文引用了其他推文，会包含在 `quote` 字段中
5. **隐私**: 无法获取已删除或私密账户的推文

## 错误处理

| 错误 | 说明 |
|------|------|
| HTTP 404 | 推文不存在或已被删除 |
| HTTP 429 | 请求过于频繁，请稍后重试 |
| Network Error | 网络连接问题 |

## Agent 使用场景

这个 skill 可以用于:
- 分析 X 上的热门话题
- 追踪特定用户的推文
- 监控竞争对手的动态
- 收集行业资讯
- 研究社交媒体趋势

## 技术实现

- 使用 FxTwitter 公共 API
- 零依赖 (仅使用 Python 标准库)
- 无需认证
- 免费使用
