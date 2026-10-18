# TB Video Queue — YT Test

Experimental test version of **TB Video Queue**.

App updated: **2026-10-19**  
Last checked with YouTube: **2026-10-18**  
Spotify episode support: **experimental**

The current version is a small static web app for building a persistent playback queue. It currently supports YouTube and experimental Spotify episode playback. The project name is intentionally broader because support for other services may be explored in the future.

This is an independent project and is not affiliated with, endorsed by, or sponsored by YouTube, Google, or Spotify.

## Experimental / Preview version

If `preview/index.html` exists, the app automatically shows a link to it.

If this README is inside a `preview/` directory, `./index.html` is the preview version for that level.

The `preview/` directory is intended as a developer-oriented experimental area. It may contain unfinished or unstable changes and may share the same browser storage as the main version.

Use it at your own risk.

Nested preview directories are allowed. For example, `preview/preview/index.html` may also be used if needed.

## Repository

Suggested repository name:

`tb-video-queue-yt-test`

## Features

- Add a supported content URL with `Ctrl+V`
- Adding a URL only adds it to the queue; playback starts with `Play` or by double-clicking a queue item
- A pasted queue item becomes selected and is scrolled into view
- `Paste URL (Ctrl+V)` button
- `Paste & Play` reads the content URL from the clipboard, adds/selects it, and starts playback
- Fetch titles with the provider's oEmbed endpoint
- No YouTube Data API key
- Persistent queue with browser `localStorage`
- Official YouTube IFrame Player
- Experimental Spotify IFrame API support for `open.spotify.com/episode/...` URLs
- Automatic playback of the next queued YouTube item
- Best-effort automatic advance for Spotify episodes (experimental)
- Saves playback positions for YouTube and Spotify episodes in browser `localStorage`
- Replaying an unfinished item resumes about 5 seconds before its saved position
- Saved playback position is removed when an item finishes normally or is removed from the queue
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

## Supported URL types

### YouTube

Examples include:

```text
https://www.youtube.com/watch?v=VIDEO_ID
https://youtu.be/VIDEO_ID
https://www.youtube.com/shorts/VIDEO_ID
https://www.youtube.com/live/VIDEO_ID
```

### Spotify episode (experimental)

Currently recognized:

```text
https://open.spotify.com/episode/EPISODE_ID
```

To get this URL from Spotify, open the episode's context menu, choose `Share`, then choose `Copy episode link`.

Spotify support is intentionally experimental. Playback behavior, autoplay, login state, and automatic queue advance may depend on Spotify's Embed behavior and browser policies.

## Playback position / resume

For YouTube and Spotify episodes, the app periodically stores the latest known playback position in browser `localStorage`.

If an unfinished item is played again, playback resumes about 5 seconds before the saved position, or from the beginning if the saved position is less than 5 seconds. This small rewind is intended to make it easier to recover context after an interruption.

The saved position for an item is removed when playback reaches the end normally or when that item is removed from the queue. `Delete Above` removes saved positions for the deleted items, and `Clear All` removes saved positions for all cleared queue items.

When the app starts, saved playback positions for items that are no longer in the queue are automatically removed.

Spotify end detection remains best-effort because the Spotify IFrame API exposes playback state updates rather than a dedicated ended event. Buffering updates are not treated as completion by this app.

## Queue copy format

`Copy Queue` uses this plain-text format:

```text
Content title
https://example.com/content-url

Another content title
https://example.com/another-content-url
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

1. The Clipboard API used by the `Paste URL`, `Paste & Play`, and `Copy Queue` buttons requires a secure context in normal web use.
2. Embedded players expect normal web origin/referrer behavior. Direct `file://` use may fail.

For local development, `http://localhost` may work because modern browsers can treat localhost as a secure context. The normal supported deployment remains HTTPS.

## Login-required content

TB Video Queue does not handle account credentials or implement its own login system.

For a service that requires login, sign in to that service normally in the **same browser profile**. When TB Video Queue opens a supported service player, the browser can use the service's existing login session.

Embedded players may behave differently depending on the service itself and on browser privacy / third-party cookie settings.

## Privacy / credentials

TB Video Queue — YT Test does not require a YouTube Data API key or Google OAuth credentials.

YouTube titles are obtained with YouTube oEmbed and playback uses the official YouTube IFrame Player.

Spotify episode titles are obtained with Spotify oEmbed and playback uses Spotify's official IFrame API.

The queue and saved playback positions are stored only in the browser's `localStorage` for the site.

### Multiple tabs

Queue data and saved playback positions stored in `localStorage` are shared between tabs of this app in the same browser profile. If they are changed in multiple tabs at the same time, later changes may overwrite earlier ones.

## Source layout

`index.html` intentionally keeps HTML, CSS, and JavaScript in one file, but the source is formatted with line breaks, indentation, section comments, and descriptive names for readability.

## License

MIT License. See `LICENSE`.
