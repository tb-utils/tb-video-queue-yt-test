# TB Video Queue — YT Test

Experimental YouTube test version of **TB Video Queue**.

App updated: **2026-10-13**  
Last checked with YouTube: **2026-10-13**

The current version is a small static web app for building a persistent YouTube playback queue. The project name is intentionally broader than YouTube because support for other video services may be explored in the future.

This is an independent project and is not affiliated with, endorsed by, or sponsored by YouTube or Google.

## Repository

Suggested repository name:

`tb-video-queue-yt-test`

## Features

- Add a YouTube URL with `Ctrl+V`
- Adding a URL only adds it to the queue; playback starts with `Play` or by double-clicking a queue item
- A pasted queue item becomes selected and is scrolled into view
- `Paste URL (Ctrl+V)` button
- Fetch video titles with YouTube oEmbed
- No YouTube Data API key
- Persistent queue with browser `localStorage`
- Official YouTube IFrame Player
- Automatic playback of the next queued video
- Fixed-height scrollable queue
- The currently playing item is scrolled into view once when playback changes
- `Scroll to Now Playing Item` scrolls the queue to the item that is currently playing
- The currently playing row is marked with `▶`
- Move items up or down
- Delete selected item
- When the queue has keyboard focus: `↑` moves the selected item up, `↓` moves it down, and `Delete` removes it
- `Delete Above` removes all items before the selected item
- `Copy Queue` copies the whole queue as plain text
- Browser standard context menu is preserved
- If the currently playing item is removed from the queue, playback stops and the player is cleared

## Queue copy format

`Copy Queue` uses this plain-text format:

```text
Video title
https://www.youtube.com/watch?v=VIDEO_ID

Another video title
https://www.youtube.com/watch?v=ANOTHER_ID
```

This is intended to remain easy to read and easy to reuse later.

## Deployment

The supported deployment is **HTTPS**, such as GitHub Pages.

### GitHub Pages

1. Create the repository, for example `tb-video-queue-yt-test`.
2. Put `index.html`, `README.md`, and `LICENSE` in the repository root.
3. Enable GitHub Pages for the repository.
4. Open the published HTTPS URL.

## Local use

Opening `index.html` directly with `file://` is **not supported**.

There are two relevant browser restrictions:

1. The Clipboard API used by the `Paste URL` and `Copy Queue` buttons requires a secure context in normal web use.
2. The YouTube embedded player expects a valid web origin / referrer. Direct `file://` use can therefore fail with YouTube player error 153.

For local development, `http://localhost` may work because modern browsers can treat localhost as a secure context. The normal supported deployment remains HTTPS.

## Login-required content

TB Video Queue does not handle account credentials or implement its own login system.

For a service that requires login, sign in to that service normally in the **same browser profile**. When TB Video Queue opens a supported service page or player, the browser can use the service's existing login session.

Embedded players may behave differently depending on the service itself and on browser privacy / third-party cookie settings.

## Privacy / credentials

TB Video Queue — YT Test does not require a YouTube Data API key or Google OAuth credentials.

Video titles are obtained with YouTube oEmbed. Playback uses the official YouTube IFrame Player.

The queue itself is stored only in the browser's `localStorage` for the site.

### Multiple tabs

Queue data stored in `localStorage` is shared between tabs of this app in the same browser profile. If the queue is changed in multiple tabs at the same time, later changes may overwrite earlier ones.

## License

MIT License. See `LICENSE`.
