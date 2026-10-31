# TB Video Queue — YT Test

Experimental test version of **TB Video Queue**.

App updated: **2026-10-29**  
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
- Adding a URL only adds it to the queue; playback starts with `Play`, or immediately when using `Paste & Play`
- A pasted queue item becomes selected and is scrolled into view
- `Paste URL (Ctrl+V)` button
- `Paste & Play` reads the content URL from the clipboard, adds/selects it, and starts playback
- Fetch titles with the provider's oEmbed endpoint
- Refresh a selected item's title with `Refresh Title`
- Refresh the title again in the background whenever playback starts, so titles that changed after being queued can be updated
- No YouTube Data API key
- Persistent queue with browser `localStorage`
- Official YouTube IFrame Player
- Experimental Spotify IFrame API support for `open.spotify.com/episode/...` URLs
- Automatic playback of the next queued YouTube item
- Best-effort automatic advance for Spotify episodes (experimental)
- Saves playback positions for YouTube and Spotify episodes in browser `localStorage`
- Shows `Resume: M:SS` (or `H:MM:SS`) in a queue row when that item has a saved resume position
- Replaying an unfinished item resumes about 5 seconds before its saved position
- Replaying a Spotify episode recreates its embed controller instead of reusing a possibly stalled one
- On page reload, the previously current queue item is selected and brought into view
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
- Queue titles and URLs can be selected and copied as normal text
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

For YouTube and Spotify episodes, the app periodically stores the latest known playback position in browser `localStorage`. When an item has a saved position, its queue row shows the effective resume point as `Resume: M:SS` or `Resume: H:MM:SS`.

If an unfinished item is played again, playback resumes about 5 seconds before the saved position, or from the beginning if the saved position is less than 5 seconds. The displayed `Resume:` value already includes this 5-second rewind, so it represents the position from which the app will normally try to resume. This small rewind is intended to make it easier to recover context after an interruption.

For Spotify episodes, each playback request recreates the Spotify embed controller. While a resume attempt is in progress, the app keeps the previously confirmed saved position until playback reaches the requested resume area. This is intended to prevent a failed reconnect from overwriting a good saved position with 0 or only a few seconds.

The saved position for an item is removed when playback reaches the end normally or when that item is removed from the queue. `Delete Above` removes saved positions for the deleted items, and `Clear All` removes saved positions for all cleared queue items.

When the app starts, saved playback positions for items that are no longer in the queue are automatically removed.

### End detection

YouTube and Spotify use different end-detection methods.

For YouTube, the app relies on the official YouTube IFrame Player API and advances the queue when the player reports `YT.PlayerState.ENDED`. The app does not infer YouTube completion from playback-position jumps or network-interruption duration.

For Spotify episodes, end detection is best-effort because the Spotify IFrame API exposes playback state updates rather than a dedicated ended event. The app automatically advances only when all of the following conditions are satisfied for the current Spotify controller:

1. `position` and `duration` are valid, `duration` is greater than zero, and the reported position is within 0.5 seconds of the reported duration.
2. The update is not reported as buffering.
3. The immediately preceding playback update was already within the final 15 seconds of the same reported duration.
4. The reported position advanced since the preceding playback update. A repeated end position by itself is not enough.
5. The amount of reported forward progress is no greater than the real time elapsed since the preceding playback update plus a 2-second tolerance. This is used to reject implausibly fast position jumps.

A sudden jump directly to the end therefore does not immediately advance the queue. This helps avoid false completion after Spotify Embed reconnect/recovery behavior and also applies to large manual seeks near the end. If playback then continues normally and reaches the end, the queue advances automatically.

An end-position report that fails the conditions above is also treated as **untrusted for resume-position storage**. It does not replace the last confirmed Spotify playback position and does not clear an active resume guard. For example, if the last confirmed position is about 770 seconds and Spotify suddenly reports `position == duration` at about 2739 seconds, the app keeps the confirmed position near 770 seconds. If the user retries playback, the normal 5-second rewind is then applied to that confirmed saved position.

The length of a network interruption by itself is not used as a rule that blocks completion. Elapsed real time is used only to judge whether the reported position change is physically plausible. A long interruption can therefore recover normally if Spotify resumes from a sensible position.

Spotify reconnect/resume behavior remains experimental, and Spotify-side playback/session failures may still require another retry or a page reload.

## Title refresh

Titles are obtained from each provider's oEmbed endpoint. A title can change after an item has been added to the queue, for example when a YouTube live stream was queued before its final title was set.

`Refresh Title` refreshes the title of the currently selected queue item. The app also starts a background title refresh whenever an item begins playback, without delaying playback while the title request completes. Adding a URL that is already present in the queue also refreshes that item's title in the background.

Title refresh uses a fresh browser request (`cache: "no-store"`), but the provider's own oEmbed service may still control what metadata is returned.

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
