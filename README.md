# EDITRIX AI — All-in-one editing MVP (v1.2)

This build combines the requested next upgrades into one starter project.

## Features implemented
- Select up to 5 video clips.
- Reorder clips using up/down controls.
- Preview the first clip.
- Add one optional audio/music track.
- Add a title/caption burned into the video.
- Choose hard cuts or a short crossfade between clips.
- Export MP4 at 1080p or 4K maximum dimensions.
- Download/share the exported MP4.

## Important limitations
This is a functional multi-clip rendering MVP, not a polished, draggable nonlinear editor like CapCut. It uses a list with reorder buttons rather than a touch-draggable timeline. Audio is a single added track; it is not yet synchronized with a visual waveform or mixable against original clip audio. Text is a single static title, not animated subtitles. Effects, filters, image overlays, stickers, keyframes, AI speech recognition, and generated images are not implemented in this build.

The crossfade mode uses FFmpeg's xfade filter. Different source dimensions, frame rates, variable frame rates, missing audio streams, or unusual codecs may require additional normalization or testing. FFmpeg drawtext needs a build with the drawtext filter and a usable font configuration. 4K export can be slow and needs substantial CPU, memory, and storage; it cannot restore source detail.

## Requirements
- Node.js 20 or later.
- FFmpeg and FFprobe installed and available on PATH.
- A computer/server for rendering.
- Phone and computer on the same Wi-Fi for local testing.
- Expo Go for preview.

FFmpeg: https://ffmpeg.org/download.html

## 1. Install mobile dependencies
In the project root:

```bash
npm install
npx expo install expo-image-picker expo-video expo-file-system expo-sharing
```

## 2. Install and start backend
Open a second terminal:

```bash
cd backend
npm install
```

Verify both commands:

```bash
ffmpeg -version
ffprobe -version
```

Then start the backend:

```bash
npm start
```

Test on the computer at `http://localhost:4000/api/health`.

## 3. Connect the phone
Find your computer's LAN IP address and change the constant near the top of `App.tsx`:

```ts
const API_BASE_URL = "http://192.168.1.20:4000";
```

Replace the example IP with your own. Both devices must be on the same Wi-Fi. Allow port 4000 on your private network firewall only. For iOS and production use, deploy an HTTPS backend; local HTTP may be restricted by platform networking rules.

## 4. Run mobile app

```bash
npx expo start
```

Scan the QR code with Expo Go.

## 5. Export a project
1. Add up to five video clips.
2. Use the up/down arrows to set clip order.
3. Optionally add one music/audio file.
4. Type a title/caption if desired.
5. Choose hard cut or fade transition.
6. Choose 1080p or 4K.
7. Tap Export complete video.
8. When rendering finishes, tap Download / Share MP4.

Optional trim start/end apply to the first clip only. Leave end blank to use the full first clip.

## 6. Build Android/iOS packages
Follow Expo's current EAS guide:
https://docs.expo.dev/build/introduction/

```bash
npx eas-cli@latest build:configure
```

Then follow EAS to build Android and iOS. iOS distribution typically requires an Apple Developer account.

## Production security checklist
This backend is intended only for local development. Before public launch:
- Add authentication and per-user authorization.
- Use HTTPS and private object storage with expiring download URLs.
- Add upload quotas, rate limits and billing/cost controls.
- Validate media by probing file contents and codec metadata.
- Run FFmpeg in isolated workers/containers with strict CPU, memory, duration and disk limits.
- Add cleanup/retention policies for uploaded and exported files.
- Add privacy policy and clear user consent for uploads.
- Do not expose this unauthenticated development server to the public internet.
