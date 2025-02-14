# MRPC
Experimental Discord Mobile Rich Presence (Android)

## How does it work?
It's pretty simple.
- Connect to the [Discord Gateway](https://discord.com/developers/docs/topics/gateway) as a normal Discord Client.
- Send [Identify](https://discord.com/developers/docs/topics/gateway#identifying) and [Update Presence](https://discord.com/developers/docs/topics/gateway#update-presence).

## Custom image support + gif
MRPC now supports setting custom image through 2 links:
- `https://media.discordapp.net` (resolves from `mp:path/to/image`)
- `https://cdn.discordapp.com` (resolves from `../../path/to/image`)

In late 2021(?), Discord allowed activity to specify media proxy image, which opens a way to set custom image.
However, there is another trick that has not been used before:

- By default, Discord parses the input as `application_assets` (unless it is set to media proxy with `mp:` prefix) 
- Setting the asset object to `test` results in `https://cdn.discordapp.com/app-assets/application-id/test.png`
- It does not verify anything there. This way, we can put `../..` to escape the path, then we can set anything such as an animated emoji right here, finally `#` at the end to exclude `.png`.
- Setting the asset object to `../../emojis/emoji-id.gif#` results in `https://cdn.discordapp.com/app-assets/application-id/../../emojis/emoji-id.gif#.png` which then gets resolved to `https://cdn.discordapp.com/emojis/emoji-id.gif`

## Notes
- MRPC parses the image link automatically, so you just need to paste the link and it will work out of the box. You can only use image link from domains listed above.
- Rate limiting is not yet handled.
- This app uses the Discord Gateway instead of OAuth2 API (which I can't even find the documentation for the `activities.write` scope), so this might not be safe. Use this at your own risk.
- Zlib compression is not yet used, due to the current using library doesn't correctly handle it(?).

## License
[Apache License 2.0](https://github.com/khanhduytran0/MRPC/blob/main/LICENSE).

## Third party libraries and their licenses
- [gson](https://github.com/google/gson): [Apache License 2.0](https://github.com/google/gson/blob/master/LICENSE).
- [Java-WebSocket](https://github.com/TooTallNate/Java-WebSocket): [MIT License](https://github.com/TooTallNate/Java-WebSocket/blob/master/LICENSE).
- [slf4j-android](https://github.com/twwwt/slf4j): [MIT License](https://github.com/twwwt/slf4j/blob/master/LICENSE.txt).

